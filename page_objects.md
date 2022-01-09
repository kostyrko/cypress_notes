1) all selectors within a page are haoused within a page object model/class

Przykład Page objectów

        class HomePage_PO {
            visitHompage() {
                cy.visit(Cypress.env("webdriveruni_homepage"));
            }

            clickOn_ContactUs_Button() {
                cy.get('#contact-us').invoke('removeAttr', 'target').click({ force: true })
            }
        }
        export default HomePage_PO;
        
        class Contact_Us_PO {
            contactForm_Submission(firstName, lastName, email, comment, $selector, textToLocate) {
                cy.get('[name="first_name"]').type(firstName);
                cy.get('[name="last_name"]').type(lastName);
                cy.get('[name="email"]').type(email)
                cy.get('textarea.feedback-input').type(comment)
                cy.get('[type="submit"]').click();
                cy.get($selector).contains(textToLocate)
            }
        }
        export default Contact_Us_PO;
        
        


Przykład zastosowania

        import Homepage_PO from '../../support/pageObjects/webdriver-uni/Homepage_PO'
        import Contact_Us_PO from '../../support/pageObjects/webdriver-uni/Contact_Us_PO'
        /// <reference types="cypress" />

        describe("Test Contact Us form via WebdriverUni", () => {
          before(function () {
            cy.fixture("example").then(function (data) {
              globalThis.data = data;
            });
          });

          beforeEach(function () {
            const homepage_PO = new Homepage_PO();
            homepage_PO.visitHompage();
            homepage_PO.clickOn_ContactUs_Button();
          });

          it("Should be able to submit a successful submission via contact us form", () => {
            cy.document().should("have.property", "charset").and("eq", "UTF-8");
            cy.title().should("include", "WebDriver | Contact Us");
            cy.url().should("include", "contactus");
            
            const contact_Us_PO = new Contact_Us_PO();
            contact_Us_PO.contactForm_Submission(Cypress.env("first_name"), data.last_name, data.email, "How can I learn Cypress?", "h1", "Thank You for your Message!");
          });

        });


#### Page object bodnar style - Page object 2nd example

cypress/support/page_objects/navigationPage.js


    function selectGroupMenuItem(groupName){
        cy.contains('a', groupName).then( menu => {
            cy.wrap(menu).find('.expand-state g g').invoke('attr', 'data-name').then( attr => {
                if( attr.includes('left')){
                    cy.wrap(menu).click()
                }
            })
        })
    }

    export class NavigationPage{

        formLayoutsPage(){
            selectGroupMenuItem('Form')
            cy.contains('Form Layouts').click()
        }

        datepickerPage(){
            selectGroupMenuItem('Form')
            cy.contains('Datepicker').click()
        }

        toasterPage(){
            selectGroupMenuItem('Modal & Overlays')
            cy.contains('Toastr').click()
        }

        smartTablePage(){
            selectGroupMenuItem('Tables & Data')
            cy.contains('Smart Table').click()
        }

        tooltipPage(){
            selectGroupMenuItem('Modal & Overlays')
            cy.contains('Tooltip').click()
        }

    }

    export const navigateTo = new NavigationPage()
    
#### usage -> testing


cypress/integration/testWithPageObjects.spec.js

    import { navigateTo } from "../support/page_objects/navigationPage"

    describe('Test with Page Objects', () => {

        beforeEach('open application', () => {
            cy.openHomePage()
        })

        ignoreOn('firefox', () => {
            it('verify navigations actoss the pages', () => {
                navigateTo.formLayoutsPage()
                navigateTo.datepickerPage()
                navigateTo.smartTablePage()
                navigateTo.tooltipPage()
                navigateTo.toasterPage()
            })
        })
    })
    
 


