---
Title: use google apps scripts to schedule notifications
Description: schedule loan checks using google scripting services and schedules.
Date: 2015/02/07
Type: Tutorial
Author: @librarieshacked
TutorialComplexity: medium
TutorialSkillsRequired: JavaScript,scripting,XML
TutorialNeeded: a google account
TutorialTime: 1 hour
TutorialType: WebDevelopment
Template: tutorialpage
---

Google is a useful place for working on collaborative documents, hosted in an environment that is fairly easy to control access to.  These include spreadsheets, docs, slides, fusion tables etc.  And also lesser known *Apps Scripts*.

The [Google Apps Script](https://developers.google.com/apps-script/) language is primarily JavaScript with a number of additions to simplify common scripting tasks, and integrate with other Google services.

For example, if you have a Google Analytics account it supports [a set of script methods](https://developers.google.com/apps-script/advanced/analytics) to allow you to easily query that analytics data.

Something like Analytics has already provided API access for a long time, but this makes accessing data from Google services a lot easier, and also removes authentication as a task.  If you write an apps script and host it on your Google account then it can be given automatic access to your Analytics data.  Similarly, it will provide easy read/write access to the data in documents and spreadsheets that are otherwise private but in the same account.

When working on scripts it also offers a fairly good development environment - allowing you to debug and work through scripts (though debugging is noticeably slow).

Scripts are able to send out emails (at a limit of 100 per day), and most importantly can be set to run as part of a schedule.  That makes it very useful as a notifications and alerts system, where otherwise any automation scripts would need to be hosted on a paid for server, or a PC that would be unlikely to be always on.

As an example, the below script could be set to run as an automated task every day to check a library account loans data, and send an email if there is one due for return within a certain time frame (e.g. within a couple of days).  It uses the following Google script additions:

- **MailApp.sendEmail** (to send our the alert email if loans are due).
- **UrlFetchApp.fetch** (to fetch data from a URL.  in this case a web service that retrieves loan data)
- **XmlService.parse** (to parse XML returned from the web service into an accessible object).

<pre class="prettyprint linenums">
<code>function CheckLoans() {

    // user acount details
    var memberId = '';
    var PIN = '';
    var emailAddress = '';

    // library details - in this case an id and web service for Wiltshire libraries
    var libraryId = '400001';
    var libraryUrl = 'https://libraries.wiltshire.gov.uk/arena.pa.palma/loans';

    // script options:
    // at less than 1 day to go we renew the item
    var daysToRenew = 1;
    // at less than 5 days to go, send an email notification
    var daysToSendEmail = 5;

    // setting up some data for the script
    // includes XML to post to the web service
    var d = new Date();
    var renewals = '';
    var today = new Date();
    var checkLoanPayload = '&lt;soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:loan="http://loans.palma.services.arena.axiell.com/" xmlns:loan1="http://axiell.com/arena/services/palma/patron/loansRequest" xmlns:util="http://axiell.com/arena/services/palma/util"&gt;&lt;soapenv:Header/&gt;&lt;soapenv:Body&gt;&lt;loan:GetLoans&gt;&lt;loan1:loansRequest&gt;&lt;util:arenaMember&gt;' + libraryId + '&lt;/util:arenaMember&gt;&lt;util:user&gt;' + memberId + '&lt;/util:user&gt;&lt;util:password&gt;' + PIN + '&lt;/util:password&gt;&lt;util:language&gt;en&lt;/util:language&gt;&lt;/loan1:loansRequest&gt;&lt;/loan:GetLoans&gt;&lt;/soapenv:Body&gt;&lt;/soapenv:Envelope&gt;';
    var renewPayload = '&lt;soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:loan="http://loans.palma.services.arena.axiell.com/" xmlns:ren="http://axiell.com/arena/services/palma/patron/renewLoansRequest" xmlns:util="http://axiell.com/arena/services/palma/util" xmlns:loan1="http://axiell.com/arena/services/palma/util/loan"&gt;&lt;soapenv:Header/&gt;&lt;soapenv:Body&gt;&lt;loan:RenewLoans&gt;&lt;ren:renewLoansRequest&gt;&lt;util:arenaMember&gt;' + libraryId + '&lt;/util:arenaMember&gt;&lt;util:user&gt;' + memberId + '&lt;/util:user&gt;&lt;util:password&gt;' + PIN + '&lt;/util:password&gt;&lt;util:language&gt;en&lt;/util:language&gt;&lt;ren:loans&gt;[[renewals]]&lt;/ren:loans&gt;&lt;/ren:renewLoansRequest&gt;&lt;/loan:RenewLoans&gt;&lt;/soapenv:Body&gt;&lt;/soapenv:Envelope&gt;';
    var loansOptions = { 'method': 'POST', 'content-type': 'application/xml; charset=utf-8', 'payload': checkLoanPayload };
    var renewOptions = { 'method': 'POST', 'content-type': 'application/xml; charset=utf-8', 'payload': renewPayload };

    // start - get loans data
    var getLoans = UrlFetchApp.fetch(libraryUrl, loansOptions);
    var responseText = getLoans.getContentText();

    // do the XML parsing to get a list of loans
    var docRoot = XmlService.parse(responseText).getRootElement();
    var ns = docRoot.getNamespace();
    var loansRequest = docRoot.getChildren('Body', ns)[0].getChildren()[0].getChildren()[0];
    ns = loansRequest.getNamespace();
    var loans = loansRequest.getChild('loans', ns);
    var loanItems = loans.getChildren();

    var emailText = 'hi libraries hacked,\n';
    var sendEmail = false;
    var renew = false;

    // loop through each loan and construct the email body (if necessary)
    for (var x in loanItems) {
        var loan = loanItems[x];
        ns = loan.getNamespace();
        var renewalDate = loan.getChild('loanDueDate', ns).getText().replace('+', 'T') + ':00.000Z';
        renewalDate = new Date(renewalDate);

        var reservedDate = new Date(loan.getChild('loanDate', ns).getText());
        var branch = loan.getChild('branch', ns).getText();

        var catalogueRecord = loan.getChildren()[1];
        ns = catalogueRecord.getNamespace();

        var title = catalogueRecord.getChild('title', ns).getText();
        var id = catalogueRecord.getChild('id', ns).getText();
        var author = catalogueRecord.getChild('author', ns).getText();
        var oneDay = 1000 * 60 * 60 * 24;
        var dateDifference = Math.ceil((renewalDate.getTime() - today.getTime()) / (oneDay));

        if (dateDifference &lt;= daysToSendEmail) {
            sendEmail = true;
            if (dateDifference &lt;= daysToRenew) {
                // it's so late we need to renew, but say this in the email.
                renew = true;
                emailText += 'items has been renewed ' + title + ', ' + author + ' reserved on ' + reservedDate + '.  remember to finish and return soon.\n';
                renewals += '&lt;loan1:id&gt;' + id + '&lt;/loan1:id&gt;'
            }
            else {
                // less than five days to go, will send an email each day.
                emailText += 'your loan of ' + title + ', ' + author + ' reserved on ' + reservedDate + ', is due back on: ' + renewalDate + '.\n';
            }
        }
    }

    // renew whatever items are due to renew
    if (renew) {
        renewPayload = renewPayload.replace('[[renewals]]', renewals);
        UrlFetchApp.fetch(libraryUrl, renewOptions);
    }

    // send out the email
    if (sendEmail) {
        MailApp.sendEmail(emailAddress, 'library notification report', emailText);
    }
}</code>
</pre>