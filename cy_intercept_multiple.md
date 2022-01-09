cy.clock() - We can "freeze" the clock and all time-related functions like setInterval, setTimeout using the command cy.clock

cy.tick() - manually advance the clock using the cy.tick command

    it('fetches from the server (spies)', () => {
      cy.clock()
      cy.intercept('GET', '/favorite-fruits').as('fruits')
      cy.visit('/fruits.html')
      // first call
      cy.wait('@fruits').its('response.statusCode').should('equal', 200)

      // 30 seconds passes and the application fetches again
      cy.tick(30000)
      cy.wait('@fruits').its('response.statusCode').should('equal', 200)
      [...]


Stubbing the server response

      it('returns different fruits every 30 seconds', () => {
        cy.clock()
        let k = 0

        // return difference responses on each call
        cy.intercept('/favorite-fruits', (req) => {
          k += 1
          switch (k) {
            case 1:
              return req.reply(['apples 🍎'])
            case 2:
              return req.reply(['grapes 🍇'])
            default:
              return req.reply(['kiwi 🥝'])
          }
        })

        cy.visit('/fruits.html')
        cy.contains('apples 🍎')
        cy.tick(30000)
        cy.contains('grapes 🍇')
        cy.tick(30000)
         cy.contains('kiwi 🥝')
    })

Every time the intercept runs, it uses up the first item from the responses array and removes it. 
After the first two times, the responses.shift() always returns undefined and we return the default kiwi fruit.

    it('returns different fruits every 30 seconds (array shift)', () => {
      cy.clock()

      // return difference responses on each call
      const responses = [
        ['apples 🍎'], ['grapes 🍇'],
      ]

      cy.intercept('/favorite-fruits', (req) => {
        req.reply(responses.shift() || ['kiwi 🥝'])
      })

      cy.visit('/fruits.html')
      cy.contains('apples 🍎')
      cy.tick(30000)
      cy.contains('grapes 🍇')
      cy.tick(30000)
      cy.contains('kiwi 🥝')
    })
    
   Source:
   

[Testing periodic network requests with cy.intercept and cy.clock combination](https://www.cypress.io/blog/2021/02/23/cy-intercept-and-cy-clock/)
