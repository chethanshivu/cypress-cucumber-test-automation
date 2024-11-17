# Cypress Cucumber BDD Automation Framework

## Overview
This automation framework combines Cypress with Cucumber for Behavior Driven Development (BDD), providing a robust solution for end-to-end testing. The framework allows writing tests in a human-readable format using Gherkin syntax while leveraging Cypress's powerful testing capabilities.

## Prerequisites
- Node.js (v14 or higher)
- npm (v6 or higher)
- Visual Studio Code (recommended)

## Installation
1. Clone the repository:
```bash
git clone <repository-url>
cd cypress-cucumber-framework
```

2. Install dependencies:
```bash
npm install
```

## Project Structure
```
├── cypress/
│   ├── e2e/
│   │   └── features/            # Feature files (.feature)
│   ├── fixtures/                # Test data files
│   ├── integration/
│   │   └── step_definitions/    # Step definition files
│   ├── pages/                   # Page Object files
│   ├── plugins/                 # Cypress plugins
│   └── support/                 # Support files and commands
├── cypress.config.js            # Cypress configuration
├── package.json
└── README.md
```

## Key Features
- BDD approach using Cucumber
- Page Object Model implementation
- Custom commands and utilities
- Test data management
- Reporting integration
- Cross-browser testing support
- CI/CD integration ready

## Writing Tests

### Feature Files
Create `.feature` files in the `cypress/e2e/features` directory:

```gherkin
Feature: Login Functionality

  Scenario: Successful login with valid credentials
    Given I am on the login page
    When I enter valid username and password
    And I click the login button
    Then I should be logged in successfully
```

### Step Definitions
Implement step definitions in `cypress/integration/step_definitions`:

```javascript
import { Given, When, Then } from '@badeball/cypress-cucumber-preprocessor';
import LoginPage from '../../pages/LoginPage';

const loginPage = new LoginPage();

Given('I am on the login page', () => {
    loginPage.visit();
});

When('I enter valid username and password', () => {
    loginPage.enterCredentials(Cypress.env('username'), Cypress.env('password'));
});
```

## Configuration
The framework can be configured through `cypress.config.js`:

```javascript
const { defineConfig } = require('cypress');
const preprocessor = require('@badeball/cypress-cucumber-preprocessor');

module.exports = defineConfig({
  e2e: {
    specPattern: 'cypress/e2e/features/*.feature',
    baseUrl: 'https://your-application-url.com',
    supportFile: 'cypress/support/e2e.js',
    setupNodeEvents(on, config) {
      preprocessor.addCucumberPreprocessorPlugin(on, config);
      return config;
    },
  },
});
```

## Running Tests

### Command Line
```bash
# Run all tests
npm run test

# Run specific feature
npm run test -- --spec "cypress/e2e/features/login.feature"

# Run tests in specific browser
npm run test -- --browser chrome

# Run tests in headless mode
npm run test -- --headless
```

### Visual Mode
```bash
npm run cypress:open
```

## Reports
The framework generates several types of reports:

1. Cucumber HTML Reports
2. Mochawesome Reports
3. JUnit XML Reports

Reports are generated in the `cypress/reports` directory after test execution.

## Best Practices

1. **Page Object Pattern**
   - Keep page-specific selectors and methods in separate page objects
   - Use meaningful names for methods and variables

2. **Test Data Management**
   - Store test data in `cypress/fixtures`
   - Use environment variables for sensitive data

3. **Step Definitions**
   - Keep steps reusable and atomic
   - Use proper annotations (@given, @when, @then)

4. **Feature Files**
   - Write scenarios in business language
   - Follow Gherkin syntax guidelines

## CI/CD Integration
The framework can be integrated with various CI/CD platforms:

```yaml
# Example GitHub Actions workflow
name: E2E Tests
on: [push]
jobs:
  cypress-run:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: cypress-io/github-action@v2
        with:
          browser: chrome
          headless: true
```

## Troubleshooting

Common issues and solutions:

1. **Test failures due to timeouts**
   - Increase defaultCommandTimeout in cypress.config.js
   - Use cy.wait() judiciously

2. **Element not found errors**
   - Check if selectors are correct and unique
   - Ensure proper wait conditions are in place

3. **Browser launch issues**
   - Update Cypress and browser versions
   - Clear Cypress cache: `npx cypress cache clear`

## Contributing
1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License
This project is licensed under the MIT License - see the LICENSE file for details.
