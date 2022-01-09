If not familaira read about: html tags, html attributes, html attibutes Values, html attribute names, test values

**HTML tags** -> div, span etc. but in the context of Angular it can by the **component selector** i.e. nb-select, sb-checked etc.

keepi in mind: cypress works with RGB color formt

Let IDE know that it's working with Cypress

    /// <reference types="cypress">  -- info dla intelisense dla vsc
    describe('Our test', ()=> {

      it('first test', ()=> {

      })

    })
    
### Locators (jQuery selector engine)

    /// <reference types="cypress">  -- info dla intelisense dla vsc
    describe('Our test', ()=> {

      it('first test', ()=> {
          // by Class name
          cy.get('.email-input')
          
          // by attribute name and value
          cy.get('[placeholder="Email"]')
          
          // by Class value
          cy.get('[class="email-input sieze-medium"]')
          
          // by Tag name and Attribute with value
          cy.get('input[placeholder="Email"]')
          
          // two attributes
          cy.get('[placeholder="Email"][type="email"]')
          
          // attribute, tag name, value, ID, Class Name
          cy.get('input[placeholder="Email"]#inputEmail.input-full-widht')
          
          // the best way -> will not change dynamically
          cy.get('[data-cy="inputEmail"]')
      })
    })
    
### cy.contains()
    
    cy.contains('Text') <- case sensetive (see how the element is in DOM - before CSS)
    
    cy.contains('[status="warning"],'Text')
   
### find() an element using child element // find() only for finding a child element within parents element

    // find a child
    cy.contains('nb-card', 'Horizontal form').find('type="email"]')

    cy.get('#inputEmail')
      .parents('form')
      .find('Button')
      // assertion
      .should('contain', 'Sign in')
      // find another
      .parents('form')
      .find('nb-checkbox')
      .click()
    
### then() & wrap()

then(jQuery format)
variable in then() method which is passed is a jQuery object (i.e. formObject below) => diffrent accertion, find(), text() from jQuery, use Chai Assertion Library

        cy.contains('Email').then( formObject => {
            const emailLabel = firstForm.find('[for="inputEmail1"]').text()
            expect(emailLabel).to.equal('Email')
        })

cy.wrap() - to convert back to cypress context/to get it the cypress way

        cy.contains('Email').then( formObject => { // jQuery object
            cy.wrap(formObject).find('[for="inputEmail1"]').should('contain', 'Email')
        })
      
### invoke() - invoke a function on the previously yielded subject.

#### getting artibute values

text example

        cy.get('[for="inputEmail"]').invoke('text').then(text => {
            expect(text).to.equal('Email address')
        })
        
 checkbox example
 
      cy.contains('nb-card')
        .find('nb-checkbox')
        .click()
        .find('.custom-checkbox')
        //.then(classValue => {
        //    expect(classValue).to.contain('checked')
        //})
        .invoke('attr', 'class')
        .should('contain', 'checked')
        
get text out of element

        cy.get('element').invoke('text')
        
       
#### Properties ('prop', 'value')

        cy.contains('nb-card').find('input').then( input => {
            cy.wrap(input).click()
            cy.get('nb-calendar-day-picker).contains('1').click()
            cy.wrap(input).invoke('prop', 'value').should('contain', 'May 17')
        })

### Radio buttons

*::{force: true} - check even if its hidden (disable cypress check for visibility)::*

*::.eq(number) - find by the index::*


#### radio buttons

         cy.contains('nb-card').find('[type="radio"]').then( radioButtons => { // multiple radio buttons/ jQuery object
            cy.wrap(radioButton)
                .first()
                .check({force: true})
                .should('be.checked')

             cy.wrap(radioButton)
                .eq(1)
                .check({force: true})

              cy.wrap(radioButton)
                .first()
                .check({force: true})
                .should('not.be.checked')

               cy.wrap(radioButton)
                .eq(2)
                .check({force: true})
                .should('not.be.disabled')
         })



#### checkbox

check() - checks the checkboxes / to uncheck use click()

        cy.get('[type="checkbox"]').check({force:true}) 
        cy.get('[type="checkbox"]').eq(0).click({force:true})
    
    
### lists and dropdowns

cy.select() - works only if the tag name === "select"

    cy.get('nb-select').click()
    // changes the skin to dark
    cy.get('.options-list').contains('Dark').click()
    // once clicks hides and the value is displayed
    cy.get('nb-select').should('contain', 'Dark)
    cy.get('.nb-layout-header nav').should('have.css', 'background-color', 'rgb(34, 34, 34)')
    
    // check multiple options
    cy.get('nav nb-select').then( dropdown => {
        cy.wrap(dropdown).click()
        cy.get('.options-list nb-option').each( (listItem, i) => {
            const itemText = listItem.text().trim() // remove spaces before if exist
            
            const colors = {
                "Light" : "rgb(100, 100, 100)",
                "Dark": "rgb(34, 34, 34)"
            }
            
            cy.wrap(listItem).click()
            cy.wrap(dropdown).should('contain', itemText)
            cy.get('.nb-layout-header nav').should('have.css', 'background-color', colors[itemText])
            if ( i < 1 ) {
                cy.wrap(dropdown).click()
            }
        })
      })
      
      
**Map all available options from select **      
      
      cy.get('#cars_list').children('option').then(options => {
         const actual = [...options].map(o => o.value)
         expect(actual).to.deep.eq(['volvo', 'saab', 'mercedes', 'audi'])
       })
       
source: [How do test all options from a normal drop down list box in Cypress
](https://stackoverflow.com/questions/53601835/how-do-test-all-options-from-a-normal-drop-down-list-box-in-cypress)

#### select nth option


Custom command - as an argument takes in a number and a previously yelded object (prevSubject)

        Cypress.Commands.add(
          'selectNth',
          { prevSubject: 'element' },
          (subject, pos) => {
            cy.wrap(subject)
              .children('option')
              .eq(pos)
              .then(e => {
                cy.wrap(subject).select(e.val())
              })
          }
        )
        
 Usage:
 
        cy.get('.parentSelector').selectNth(2)



source: [How to select nth item inside select element in cypress](https://stackoverflow.com/questions/50750956/how-to-select-nth-item-inside-select-element-in-cypress)
    
### table


    cy.get('tbody').contains('tr', 'Arry').then( tableRow => {
        cy.wrap(tableRow).find('.nb-edit').click()
        cy.wrap(tableRow).find('[placeholder="Age"]').clear().type('25')
        cy.wrap(tableRow).find('.check-mark').click()
        cy.wrap(tableRow).find('.nb-edit').click()
        cy.wrap(tableRow).find('td').eq(6).should('contain', '25')
    })
     
    // table columns
    cy.get('tbody tr').first().find('td').then( tableColumns => {
        cy.wrap(tableColumns).eq(2).should('contain', 'Artem')
        cy.wrap(tableColumns).eq(3).should('contain', 'Bondar')
     })

    // use search option to search for age
    // use cy.wrap to use each() [array contains results possible to find and those that can not be found]
    const age = [20, 30, 40, 200]

    cy.wrap(age).each( age => {
        cy.get('thead [placeholder="Age"]').clear().type(age)
        cy.wait(500) // delay due to angular 
        cy.get('tbody tr').each( tableRow => {
            if(age == 200){
                cy.wrap(tableRow).should('contain', 'No data found')
            } else {
                cy.wrap(tableRow).find('td').eq(6).should('contain', age)
            }
        })
    })

### date picker


    // function placed outside of the test body
    function selectDayFromCurrent(day){
    
            let date = new Date() // current date object
            date.setDate(date.getDate() + day) // sets today's date to future date by adding + x days
            let futureDay = date.getDate() // gets date set above to future
            let futureMonth = date.toLocaleString('default', {month: 'short'}) // short format for month
            
            let dateAssert = futureMonth+' '+futureDay+', '+date.getFullYear() // assert if the month is a future month
            
            cy.get('nb-calendar-navigation').invoke('attr', 'ng-reflect-date').then( dateAttribute => {
                // if month is not a future month click next on date picker
                if(!dateAttribute.includes(futureMonth)){
                    cy.get('[data-name="chevron-right"]').click()
                    selectDayFromCurrent(day) // function calling itsself (loop)
                } else {
                    cy.get('nb-calendar-day-picker [class="day-cell ng-star-inserted"]').contains(futureDay).click()
                }
            })
            return dateAssert // returns date to check it
      }
      
      cy.contains('nb-card', 'Common Datepicker').find('input').then( input => {
            cy.wrap(input).click()
            let dateAssert = selectDayFromCurrent(2)
            cy.wrap(input).invoke('prop', 'value').should('contain', dateAssert)
            cy.wrap(input).should('have.value', dateAssert)
        })
      
     
### dialog box (browser dialog box) => cy.stub()
        
        // !! browser dialog box
        const stub = cy.stub() // new object created for stub
        cy.on('window:confirm', stub) // when the event fast fired assign it to a stub object (.getCall(0))
        cy.get('tbody tr').first().find('.nb-trash').click().then(() => {
            expect(stub.getCall(0)).to.be.calledWith('Are you sure you want to delete?')
        })
        
        
        cy.get('tbody tr').first().find('.nb-trash').click()
        cy.on('window:confirm', () => false)

### [assertions => Chai assertion library => BDD, TDD, Chai-jQuery](https://docs.cypress.io/guides/references/assertions#Chai)

        // using invoke
        cy.wrap(input).invoke('prop', 'value').should('contain', dateAssert)
        // using only should
        cy.wrap(input).should('have.value', dateAssert)




    


Source:

[Postavshik/firstTest.spec.js](https://github.com/Postavshik/ngx-cypress-test/blob/UdemyClass/cypress/integration/firstTest.spec.js)
