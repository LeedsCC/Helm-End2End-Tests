# Helm-End2End-Tests

This repository contains the tools for e2e/Browser Automated testing of the Helm PHR. This type of test gives possibility to check working of our application in browser. 
We can open pages and simulate users actions. This involves testing the three key components supported by Ripple Foundation, namely; PulseTile, QEWD.js and EtherCIS from the user interface down. The test material contains automated test scripts that are set up as a job and trigger user like behaviour at the browser, which simulates user driven testing from frontend to backend. 

For more information on end to end testing, please see 
https://www.softwaretestinghelp.com/what-is-end-to-end-testing/

# Instruction
**For Windows:**

* Install Chrome https://www.google.com/chrome/browser/desktop/index.html
* Install JDK http://www.oracle.com/technetwork/java/javase/downloads/index.html, latest will do
* Install node.js and npm https://www.npmjs.com/get-npm?utm_source=house&utm_medium=homepage&utm_campaign=free%20orgs&utm_term=Install%20npm 
* Install nightwatch http://nightwatchjs.org/gettingstarted#installation 
* Download selenium standalone server http://selenium-release.storage.googleapis.com/index.html 
* Download chrome driver https://sites.google.com/a/chromium.org/chromedriver/downloads 
* Clone the repository to your machine
* Configure nightwatch.json file:
    * Set path to selenium-standalone-server.jar and chromedriver.exe in nightwatch.json selenium > server_path
    * Set path to chromedriver.exe in nightwatch.json selenium > cli_args > webdriver.chrome.driver
* Run 'npm install' command from parent catalog to install the dependencies (nightwatch-html-reporter)
* Warning nightwatch-html-reporter uses pug library which has a known issue that should be fixed manually:
* Fix nightwatch-html-reporter
    * Open node_modules\nightwatch-html-reporter\node_modules\pug\node_modules\pug-code-gen\index.js
    * Scroll to line 813 'attributeBlocks(attributeBlocks) {'
    * Make it 'attributeBlocks:function(attributeBlocks) {'
    * Yes, that's dull, but there is a known issue and this is a workaround. Maybe later versions of reporter would use later version of pug and this step would be redundant.


**For macOS:**

* Install Chrome https://www.google.com/chrome/browser/desktop/index.html
* Install JDK http://www.oracle.com/technetwork/java/javase/downloads/index.html, latest will do
* Install node.js and npm https://www.npmjs.com/get-npm?utm_source=house&utm_medium=homepage&utm_campaign=free%20orgs&utm_term=Install%20npm 
* Install nightwatch http://nightwatchjs.org/gettingstarted#installation 
* Download selenium standalone server http://selenium-release.storage.googleapis.com/index.html (latest is ok) Or brew install selenium-server-standalone selenium-server -port 4444
* Download chrome driver https://sites.google.com/a/chromium.org/chromedriver/downloads
* Clone the repository to your machine
* Configure nightwatch.json file:
    * Set path to selenium-standalone-server.jar in "server-path"
    * Set path to chromedriver in cli_args > 'webdriver.chrome.driver' 
* Run 'npm install' command from parent catalog to install the dependencies (nightwatch-html-reporter)
* Warning nightwatch-html-reporter uses pug library which has a known issue that should be fixed manually:
* Fix nightwatch-html-reporter
    * Open node_modules\nightwatch-html-reporter\node_modules\pug\node_modules\pug-code-gen\index.js
    * Scroll to line 813 'attributeBlocks(attributeBlocks) {'
    * Make it 'attributeBlocks:function(attributeBlocks) {'
    * Yes, that's dull, but there is a known issue and this is a workaround. Maybe later versions of reporter would use later version of pug and this step would be redundant.

# nightwatch.json
Is a config file. See for full details http://nightwatchjs.org/gettingstarted/#settings-file 
The important part besides paths to chrome driver and selenium server is the ability to set several environments. Environments contain information about your app url, selenium server url (which shouldn't be changed unless you are going to use remote servers like selenium grid or cloud solutions), whether to do screenshots and where to put those, etc. 

Besides this file is used to set:
1) launch URL of tested application;
2) login and password for authorization;
3) other information, which should be used in tests.

You can easily run tests on by using the following command: **nightwatch --env=helm**

# Jenkins
Jenkins is the open-source automation server, which can be used for automated testing of software. Among others it can be used for browser automated testing.

For more information about Jenkins please see:
https://jenkins.io/

* Log in with your account
* In the center of the screen you see the table of the jobs with your job
* The job pulls code from Github, runs Nightwatch test and sends report
* You can run the job by clicking on run button
* After the job is done, it will send an email with test result
* You can see job details by clicking on its name
* You can edit job settings by going to job's details and clicking Configure in the menu:
    * You can set uo/change job schedule in Triggers section. Cron notation is used, https://crontab.guru
    * You can add/edit job's email recipient list in Post-build section > Editable Email Notifications > Project Recipient List