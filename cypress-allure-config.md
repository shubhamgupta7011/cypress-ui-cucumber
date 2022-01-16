STEP 1 INSTALLATION

npm i --save cypress @shelex/cypress-allure-plugin allure-commandline

STEP 2 CONFIGURATION
a. In cypress/plugin/index.js

/// <reference types="@shelex/cypress-allure-plugin" />

const allureWriter = require("@shelex/cypress-allure-plugin/writer");

module.exports = (on, config) => {
  allureWriter(on, config);
  return config;
};

b. In cypress/support/index.js

import '@shelex/cypress-allure-plugin';

STEP 3 Add Terminal Commands inside scripts object available into package.json file

"scripts": {
    "cy:run": "cypress run --env allure=true",
    "allure:report": "allure generate allure-results --clean -o allure-report",
    "allure:clear": "if exist allure-results rmdir /q /s allure-results && if exist allure-report rmdir /q /s allure-report && if exist cypress\\screenshots rmdir /q /s cypress\\screenshots && if exist cypress\\videos rmdir /q /s cypress\\videos",
    "pretest": "npm run allure:clear",
    "test": "npm run cy:run || npm run posttest",
    "posttest": "npm run allure:report"
  }

STEP 4 Open Allure Report (index.html) using Live Server







    "clean:reports": "rm -R -f cypress/output/reports && mkdir cypress/output/reports && mkdir cypress/output/reports/mochareports",
    "pretest": "npm run clean:reports",
    "scripts": "cypress run",
    "combine-reports": "mochawesome-merge cypress/output/reports/mocha/*.json > cypress/output/reports/mochareports/report.json",
    "generate-report": "marge cypress/output/reports/mochareports/report.json -f report -o cypress/output/reports/mochareports",
    "posttest": "npm run combine-reports && npm run generate-report",
    "test": "npm run scripts || npm run posttest"