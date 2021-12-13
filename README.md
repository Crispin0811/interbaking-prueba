
# Mobile, API and Web test automation
  
Mobile, API and Web automation framework based on Cucumber - WebdriverIO - NodeJS - Typescript - Axios - Allure Reports
  
## Table of contents for web automation configuration

*  [Pre-requisites](#Pre-requisites)
    +  [Installling NodeJS - JDK(#Configure-environment-variable)
	+  [Configure environment variable](#Configure-environment-variable)
+  [First time run](#First-time-run)

*  [Folders structure](#Folders-structure)
	+  [Config](#Config)
	+  [features](#features)
	+  [pages: Using Page Object Model Pattern](#pages)
	+  [stepDefinitions](#stepDefinitions)
	+  [scripts](#scripts)
	+  [commons](#commons)
+  [package.json](#package.json)

+  [Pipeline execution](#Pipeline-execution)


## Getting started
### Pre-requisites

- Latest version of Node.js from the [official website](https://nodejs.org/) or using [nvm](https://github.com/creationix/nvm) (nvm approach is preferred).
- Go to [official website JDK 1.8 ](https://www.oracle.com/sa/java/technologies/javase/javase-jdk8-downloads.html), go to *Java SE Development Kit* download and install the last version.

### Configure environment variable

1. Right click the Computer icon.
2. Choose Properties from the context menu.
3. Click the Advanced system settings link.
4. Click Environment Variables. 
5. In the section System Variables, create or edit **"JAVA_HOME"** variable and set path of java. For example
```
Name  : JAVA_HOME
Value : C:\Program Files\Java\jdk1.8.0_241
```
# First time run

As root or admin.

```

$ npm install

$ npm run clean-build

```

### Run tests by tag @run
With an emulator open.

```
For run on chome

$ npm run chrome-test

For run on firefox

$ npm run Firefox-test

```

# **Web Config**


We have the file **wdio.chrome.config.js**: with configurations to run web tests on Chrome:


- **specs**: List of cucumber features to execute.
-  **reporters/reporter options:** Specifies the reporter to use, we use Allure Report.
-  **host/port/path:** Configuration of chrome server.
-  **maxInstances:** Maximus instances per execution, by default we must execute with 1 for mobile. 
-  **sync:** Always in true.
-  **capabilities:** set of options related to browser.
   - **browserName:** 'chrome'
   - **protocol:** 'http',
   - **hostname:** 'localhost',
   - **port:** 7676,
   - **path:** '/',	
   - **maxInstances:** 2
  
- **services:** chromedriver service.
 
- **logLevel:** We can use debug to log all the actions on selenium or silent mode only to log final results.
- **framework/cucumberOpts:** We use Cucumber framework to execute.
  - On **cucumberOpts.require** we specify the steps of .js files to exectute of the folder **stepDefinitions**.
  

We have the file **wdio.firefox.config.js**: with configurations to run web tests on Firefox:


- **specs**: List of cucumber features to execute.
-  **reporters/reporter options:** Specifies the reporter to use, we use Allure Report.
-  **host/port/path:** Configuration of chrome server.
-  **maxInstances:** Maximus instances per execution, by default we must execute with 1 for mobile. 
-  **sync:** Always in true.
-  **capabilities:** set of options related to browser.
   - **browserName:** 'firefox'
   - **protocol:** 'http',
   - **hostname:** 'localhost',
   - **port:** 7676,
   - **path:** '/',	
   - **maxInstances:** 2
  
- **services:** geckodriver service.
 
- **logLevel:** We can use debug to log all the actions on selenium or silent mode only to log final results.
- **framework/cucumberOpts:** We use Cucumber framework to execute.
  - On **cucumberOpts.require** we specify the steps of .js files to exectute of the folder **stepDefinitions**.
 **List of hooks of Cucumber we use:**

- **onPrepare:** Actions to to before de begin of the excecution.
- **afterCommand:** Actions to do after a command is executed, for example after a click we want to take a screenshot.
- **onComplete:** Actions to do after all test had been executed.
- **beforeScenario:** Actions to do before a scenario is executed. For example: we use **browser.reset();** to prepeare the initial conditions.
- **afterScenario:** Actions to do after a scenario was executed. For example: **browser.closeApp()**; to close de app.

We can use differents hooks, please visit [WebDriver IO Hooks](https://webdriver.io/docs/options.html#hooks) for more details.

On this folder we have the connections files to the database.

We have the **dbconfig.js** file with the database credentials.

In this file with have the conection information: 
	
- **user**          : the username that we used for database connection. Example: "user_example",
- **password**      : the password that we used for database connection. Example: "password_example",
- **connectString** : the connect String that we user for database connection. Example: "192.168.218.52:1521/dbexample"

To test the connection use on console the next command: node models/connectionExample.js.

# **features**


On this folder we have the list of cucumber .feature files group by functionalities.

# **pages**

On this folder we have the definitions of each view of our application using  Page Object Model pattern to define using Typescript.

The basic structure of each POM must have:
Filename: **viewToModel.ts**

```Typescript
    export  class  ViewToModel {
        // List of elements of the view
        private  title_page_label: string;
        private  dni_input: string;
        private  password_input: string;
        private  login_btn: string;

        // Basic constructor with the definition of each element using accesibility id, id or xpath. Try to avoid large or positional xpath.
		constructor() {
-	        this.title_page_label = '~main-title-label';
	        this.dni_input = '~phone-number-input';
			this.password_input = '~login-password-input';
			this.login_btn = '~login-button';
	    }
	    // First method to execute when the view is loaded to verify that we are on the page in fact. We should return a value to validate
	    public validatePage(): string {
		    browser.$(this.dni_input).waitForExist();
			browser.$(this.password_input).waitForExist();
		    browser.$(this.login_btn).waitForExist();
			return browser.$(this.title_page_label).getText();
	    }
	    
	    // Getters of each element of the view
		public getDniInput(): WebdriverIO.Element {
		    browser.$(this.dni_input).waitForExist();
		    return browser.$(this.dni_input);
	    }
    
		public getPaswordInput(): WebdriverIO.Element {
		    browser.$(this.password_input).waitForExist();
		    return browser.$(this.password_input);
	    }

		public getLoginBtn(): WebdriverIO.Element {
		    browser.$(this.login_btn).waitForExist();
		    return browser.$(this.login_btn);
	    }
	}
```

# **stepDefinitions**

On this folder we have to create the steps per functionality out set of scenarios defined on our .feature files.

**For example**:
  We have a feature file with this definition to execute with a set of 2 groups of data.

**onboarding.feature**

```Gherkin
@onboarding
Feature: Onboarding

As a person, I want to be a app/webpage user.

@onboarding_happy_path @run
Scenario Outline: <case> - Onboarding to app/webpage Happy Path
	Given I am not an app/webpage user
	When I complete my email from "<domain>"
	When I complete my name and lastname
	When I complete my document number
	When I complete with my mobile number
	When I complete the verification code
	When I complete the password with "<password>" and repeat with "<password>"
	Then I finished the registration to app/webpage

	Examples:

	| case | domain          | password |
	| 1    | usuario1.ccc    | 024680   |
	| 2    | usuario2.ccc    | 024680   |
```

For each **Given**, **When**, **Then** step we have to create a step to do the behavior that happen on the application/webpage.

**For example:**
On our **onboardingSteps.ts** file, we will import our pages defined to create a test.

```Typescript
import { expect } from  'chai'; // to make assertion
import { Given, Status, Then, When } from  'cucumber'; // Cucumber step definitons
import  faker  from  'faker'; // Useful to generate random data
import { Constants } from  '../commons/constants';
import { BasePermissionsPageObject } from  '../pages/basePermissionsPageObject'; // Page Object
import { OnboardingEnterCodePageObject } from  '../pages/onboardingEnterCodePageObject';

//  We create our Page Objects and validate that we stay on them, also we get an element of that view and click on it
Given(/^I am not a app/webpage user$/, () => {
	const onboarding_welcome_page: OnboardingWelcomePageObject = new  OnboardingWelcomePageObject();
	expect(onboarding_welcome_page.validatePage()).to.be.equal(Constants.ONBORDING_WELCOME_1_TITLE);
	onboarding_welcome_page.getRegisterBtn().click();
});

// We recibe data from parameters and set it on the view
When(/^I complete email with "(.*?)"$/, (mail: string) => {
	const  onboarding_enter_email_page: OnboardingEnterEmailPageObject = new  OnboardingEnterEmailPageObject();
	expect(onboarding_enter_email_page.validatePage()).to.be.equal(Constants.ONBOARDING_TITLE_INPUT_EMAIL);
	onboarding_enter_email_page.getEmailInput().setValue(mail);
	press_enter();
});
```
# **scripts**

On this folder we have several file useful to excecute on GitHub Action.

- runTest.sh: Install and run our set of test.
- reportTest.sh: Generate allure report with evidence of the execution.

# **commons**

On this folder we have a few utility classes for example: **constants.ts**, **utils.ts**.
  



# **package.json**

Scripts desctiptions:


- `clean-build`: this script transpiles Typescript code and we need to excecute after we make a change on a ***.ts** file.
-  `chrome-test`: this run all test cases on every .feature file that we have on our **config/wdio.chrome.config.js** on field specs and have to be releated to the stepDefinition files on cucumberOpts.require from the same config file.
-  `chrome-test-tag`: The same that `chrome-test` but with this script we only run the **scenarios** or **features** with **@run** tag.
-  `firefox-test`: this run all test cases on every .feature file that we have on our **config/wdio.firefox.config.js** on field specs and have to be releated to the stepDefinition files on cucumberOpts.require from the same config file.
-  `firefox-test-tag`: The same that `firefox-test` but with this script we only run the **scenarios** or **features** with **@run** tag.
- `clean-report`: We generate the allure reports after an excecution.

# **Contributing**

  

### Commits:

  

For our commits message we use [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) specification which provides an easy set of rules for creating an explicit commit history adding readable meaning to commit messages.

  

> See [Conventional Commits full specification](https://www.conventionalcommits.org/en/v1.0.0/#specification).

  

#  **Process**

  

1. Fork from `master` branch.

2. Create your branch with the associated Jira ticket  (`git checkout -b NEW-####`)

3. Commit your changes (`git commit -am 'feat(feature):'`)

4. Push to the branch (`git push origin NEW-####`)

5. Create new Pull Request
 