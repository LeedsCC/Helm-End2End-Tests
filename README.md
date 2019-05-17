# Helm-End2End-Tests

This repository contains the tools for e2e/Browser Automated testing of the Helm PHR. This type of test gives possibility to check working of our application in browser. 
We can open pages and simulate users actions. This involves testing the three key components supported by Ripple Foundation, namely; PulseTile, QEWD.js and EtherCIS from the user interface down. The test material contains automated test scripts that are set up as a job and trigger user like behaviour at the browser, which simulates user driven testing from frontend to backend. 

For more information on end to end testing, please see 
https://www.softwaretestinghelp.com/what-is-end-to-end-testing/

# Instruction
**For Windows:**

* Install Chrome https://www.google.com/chrome/browser/desktop/index.html
* Install JDK http://www.oracle.com/technetwork/java/javase/downloads/index.html, latest will do
* Install node.js and npm https://www.npmjs.com/get-npm?utm_source=house&utm_medium=homepage&utm_campaign=free%20orgs&utm_term=Install%20npm (checked for node version 0.10.22 and npm version 1.3.14)
* Install nightwatch http://nightwatchjs.org/gettingstarted#installation (checked for version 0.9.16)
* Download selenium standalone server http://selenium-release.storage.googleapis.com/index.html (checked for version 3.8.0)
* Download chrome driver https://sites.google.com/a/chromium.org/chromedriver/downloads (checked for version 2.34)
* Clone the repository to your machine
* Configure nightwatch.json file:
    * Set path to selenium-standalone-server.jar and chromedriver.exe in nightwatch.json selenium > server_path
    * Set path to chromedriver.exe in nightwatch.json selenium > cli_args > webdriver.chrome.driver
* Run 'npm install' command from parent catalog to install the dependencies (nightwatch-html-reporter)
* Warning nightwatch-html-reporter (version 2.0.4) uses pug library which has a known issue that should be fixed manually:
* Fix nightwatch-html-reporter
    * Open node_modules\nightwatch-html-reporter\node_modules\pug\node_modules\pug-code-gen\index.js
    * Scroll to line 813 'attributeBlocks(attributeBlocks) {'
    * Make it 'attributeBlocks:function(attributeBlocks) {'
    * Yes, that's dull, but there is a known issue and this is a workaround. Maybe later versions of reporter would use later version of pug and this step would be redundant.
* Run tests from parent catalog command line with command 'nightwatch'
* When the tests are finished, the report will be displayed in your browser
* You can find screenshots of failed tests in reports\screenshots


**For macOS:**

* Install Chrome https://www.google.com/chrome/browser/desktop/index.html
* Install JDK http://www.oracle.com/technetwork/java/javase/downloads/index.html, latest will do
* Install node.js and npm https://www.npmjs.com/get-npm?utm_source=house&utm_medium=homepage&utm_campaign=free%20orgs&utm_term=Install%20npm (checked for node version 8.9.3 and npm version 5.6.0 )
* Install nightwatch http://nightwatchjs.org/gettingstarted#installation (checked for version 0.9.16)
* Download selenium standalone server http://selenium-release.storage.googleapis.com/index.html (latest is ok) Or brew install selenium-server-standalone selenium-server -port 4444 (I use selenium-standalone v.3.8.1)
* Download chrome driver https://sites.google.com/a/chromium.org/chromedriver/downloads
* Clone the repository to your machine
* Configure nightwatch.json file:
    * Set path to selenium-standalone-server.jar in "server-path": (i.e. "server_path" : "/usr/local/Cellar/selenium-server-standalone/3.8.1/libexec/selenium-server-standalone-3.8.1.jar")
    * Set path to chromedriver in cli_args > 'webdriver.chrome.driver' (i.e. "webdriver.chrome.driver" : "/Users/{user}/pulse_tile/chromedriver" )
* Run 'npm install' command from parent catalog to install the dependencies (nightwatch-html-reporter)
* Warning nightwatch-html-reporter (version 2.0.4) uses pug library which has a known issue that should be fixed manually:
* Fix nightwatch-html-reporter
    * Open node_modules\nightwatch-html-reporter\node_modules\pug\node_modules\pug-code-gen\index.js
    * Scroll to line 813 'attributeBlocks(attributeBlocks) {'
    * Make it 'attributeBlocks:function(attributeBlocks) {'
    * Yes, that's dull, but there is a known issue and this is a workaround. Maybe later versions of reporter would use later version of pug and this step would be redundant.
* Run tests from parent catalog command line with command: 'nightwatch'


# nightwatch.json
Is a config file. See for full details http://nightwatchjs.org/gettingstarted/#settings-file 
The important part besides paths to chrome driver and selenium server is the ability to set several environments. Environments contain information about your app url, selenium server url (which shouldn't be changed unless you are going to use remote servers like selenium grid or cloud solutions), whether to do screenshots and where to put those, etc. 

There is a dev Helm version of the app, that is tested by End2End tests: http://dev.ripple.foundation:8000/

You can easily run tests on by using the following command: **nightwatch --env=helm**

Version switch is implemented with API calls, see globals.js. So if you want to add a new env with switching versions, make sure to set

* "version_switch_host": "dev.ripple.foundation", - host of API calls
* "version_switch_init_path": "/api/initialise", - to provide authorization
* "version_switch_path": "/api/ui/version_name"

# Jenkins
Jenkins is the open-source automation server, which can be used for automated testing of software. Among others it can be used for browser automated testing.

For more information about Jenkins please see:
https://jenkins.io/

* Go to http://138.68.171.243:8080
* Log in with your account
* In the center of the screen you see the table of the jobs. There are such jobs:
    * Showcase Nightwatch Tests
    * Dev React Nightwatch Tests
    * Dev Angular Nightwatch Tests
    * Helm Nightwatch Tests
* Each job pulls code from Github, runs Nightwatch tests and sends reports.
* You can run the job by clicking on run button in the very right
* Also jobs are run by cron timer:
    * Showcase Nightwatch Tests is run Mon-Fri at 7.30 AM London time
    * Dev React Nightwatch Tests is run Mon-Fri at 5.30 AM London time
    * Dev Angular Nightwatch Tests is run Mon-Fri at 6.30 AM London time
    * Helm Nightwatch Tests is run Mon-Fri at 4.30 AM London time
* After job is done, it will send out a email with test results.
    * Test report and build log are attached to the email
    * Email is sent from gmail acc jenkins.ripple@gmail.com pass: ripple.foundation
* Jobs are set to wait for each other to finish, so no parallel runs could happen.
* You can see job details by clicking on its name
* You can browse working directory of the job by going to job's details and clicking Working Directory in the menu on the left. In working directory there is pulled source code and reports, screenshots of the latest build
* You can edit jobs settings by going to job's details and clicking Settings in the left meny.
    * You can change job schedule in Triggers section. Cron notation is used, https://crontab.guru
    * You can edit job's email recipient list in Post-build section > Editable Email Notifications > Project Recipient List
    * Don't change things unless you know what you are doing
* You can change Jenkins settings by going to Jenkins home page and clicking 'Manage Jenkins' in the left menu
    * You can create users by going to Manage Jenkins menu, scrolling to the bottom and selecting Manage Users, further is intuitive
    * You can change mail setting by going to Manage Jenkins menu > Configuration, scrolling to the bottom and editing Extended Email Notification settings
* When in doubt - click on a ? icon, it's often placed near changeable things
* Last, but not least. Each jenkins job copies /home/rippletest/nightwatch.json to it's workspace. This is required to preserve path relative to jenkins machine. So if you change something in nightwatch.js file in code you should ssh to jenkins machine and do the changes in mentioned file too.
