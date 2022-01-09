### cy.intercept()

HTTP requests - manage the behavior of these requests

Waiting on HTTP requests to complete before executing commands.
Making assertions and modifying (statically or dynamically):
the request made by your front-end application.
the response from your back-end service.
Mocking all or some of your backend API by stubbing out responses.

    beforeEach(() => {
        cy.viewport(viewport)
        cy.intercept('GET', '/xxx/xxx_data?page=1', { fixture: 'xxx/xxx_data.json' })
        
        
 ###cy.intercept() + cy.wait()
 
Listen to GET to comments/1

    cy.intercept('GET', '**/comments/*').as('getComment')


we have code that gets a comment when

    // the button is clicked in scripts.js
    cy.get('.network-btn').click()

wait for GET comments/1

    cy.wait('@getComment').its('response.statusCode').should('be.oneOf', [200, 304])
    
    
tip: log the request object to see everything it has in the console

    cy.get('@post').then(console.log)

### cy.wait()

cy.wait can be used with additional arguments

if looking for a certain request, we can use an array i.e. in which we can pass multple requests to wait for, it can even be the same request if it will be called multiple times

    cy.wait(['@req1','@req2'])
    
    // will take 5 second to wait for 2nd request to happen
    cy.wait(['@req1','@req1'])
    
cy.wait() i a time between the request is longer a specified time between requests is possible to be passed in as an object

    cy.wait(['@req1','@req1'], {timeout: 2000})
    
If looking to match all request cy.get() is the one to catch them all

    cy.get('@req1.all').then(....


Źródło:
[intercept - cypress.io](https://docs.cypress.io/api/commands/intercept)

[example.cypress.io/commands/waiting](https://example.cypress.io/commands/waiting)

[Asserting Network Calls from Cypress Tests](https://www.cypress.io/blog/2019/12/23/asserting-network-calls-from-cypress-tests/)
