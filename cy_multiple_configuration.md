prostym rozwiązaniem jest dodanie zmiennej środowiskowej w cypress.json (someOtherVariable)

    {
      "baseUrl": "http://clientA.internalserver.co.uk/",
      "env": {
        "someOtherWebsite": "http://clientB.internalserver.com/"
      }
    }

Odwołanie się do niej wygląda w sposób następujący

    cy.visit(Cypress.env("someOtherWebsite"))
    
Istnieje również możliwość tworzenia wielu plików konfigurujących cypress.json np w folderze o nazwie config gdzie pliki posiadają nazewnictwo config.someWebsiteA.json, config.someWebsiteA.json etc.


      {
        "baseUrl": "http://clientB.internalserver.co.uk/",
        "env": {
          "someVariable": "Bar"
        }
      } 

index.js (Folder Plugins)

    const path = require("path");
    const fs = require("fs-extra");

    function getConfigurationFileByEnvName(env) {
      const fileLocation = path.resolve("cypress/config", `config.${env}.json`);
      return fs.readJson(fileLocation);
    }

    module.exports = (on, config) => {  
      const envFile = config.env.configFile || "local";
      return getConfigurationFileByEnvName(envFile);
    };


Użytkowanie Cypressa poprzez podanie odwołania do pliku konfiguracyjnego

    cypress open --env configFile=someWebsiteA

[Configuring Cypress To Run On Different Environments](https://ahmed-alsaab.medium.com/configuring-cypress-to-run-on-different-environments-7ae323bb3c86)

[Supporting multiple configurations in Cypress](https://yer.ac/blog/2020/02/07/supporting-multiple-configurations-in-cypress/)

[How to configure Cypress for multiple testing environments](https://www.bornfight.com/blog/how-to-configure-cypress-for-multiple-testing-environments/)

[cypress.json - configuration](https://docs.cypress.io/guides/references/configuration#cypress-json)
