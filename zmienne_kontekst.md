### Przypadek pojedynczego tekstu/funkcji

Najprostszym sposobem na tworzenie zmiennej w ramach pojedynczego testu jest korzystanie z tzw. domknięć - funkcji zagnieżdżonych
Gdzie funkcja zagnieżdżona posiada dostęp do zadeklarowanej zmiennej w ramach "rodzica"

    it.only("should delete shipping not in planned route", () => {
          cy.get('tbody > :nth-child(1) > :nth-child(4)').then($elem =>{
              const driver = $elem.text();
              cy.get(':nth-child(1) > .d-flex > .btn-primary').click();
              cy.get('tbody > :nth-child(1)').should('exist');
              cy.get('tbody > :nth-child(1) > .font-weight-normal').then($elem =>{
                  const id = $elem.text();
                  cy.get(':nth-child(1) > .d-flex > .btn').click();
                  cy.get('tbody').should('not.contain', id);
                  cy.get('.btn').contains('Zapisz').click();
                  // Trasa o dacie wyjazdu 2021-05-20 dla pojazdu RA476SC prowadzonego przez Hannah Grimes została zmieniona. 
                  cy.get('.toast-body').should('contain', driver);
              })
          })
      });


### Kontekst grupy testów

Tymaczsem kontekst dla grypy testów najlepiej wytwarzać przy pomocy metody aliasu as()

    describe('parent', () => {
          beforeEach(() => {
            cy.wrap('one').as('a')
          })

          context('child', () => {
            beforeEach(() => {
              cy.wrap('two').as('b')
            })

            describe('grandchild', () => {
              beforeEach(() => {
                cy.wrap('three').as('c')
              })

              it('can access all aliases as properties', function () {
                expect(this.a).to.eq('one') // true
                expect(this.b).to.eq('two') // true
                expect(this.c).to.eq('three') // true
              })
            })
          })
        })

### environment variable

cypress.json

        {
         "env" {
            "username": "CypressTester"
        }

usage:

    >> Cypress.env('username') <<
    
    
    Cypress.env('apiUrl')+'api/users/login'
    
    const userCredentials = {
        "user" : {
            "email": Cypress.env('username')
        }
    }


### plugins - switching between different config files

[Extending the Cypress Config File](https://www.cypress.io/blog/2020/06/18/extending-the-cypress-config-file/)


#### źródło

[docs.cypress.io - Variables and Aliases](https://docs.cypress.io/guides/core-concepts/variables-and-aliases#Return-Values)
