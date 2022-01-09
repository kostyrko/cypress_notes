Cypress nie powstał w kontekście visual testing i dlatego, sprawdzenie czy coś jest widoczne musi odbywać się w sposób pośredni

    let initialPosition
    cy.get('[data-cy="XYZ"]').then($xyz => {
      initialPosition = $xyz.position()
    })

    cy.scrollTo('bottom')
    
    cy.get('[data-cy="LimitedCreditsAnnouncement"]').should($xyz => {
      expect($xyz.position()).deep.equal(initialPosition)
    })
    
    
 Albo poprzez odwołanie się do css
 
    cy.get('[data-cy="XYZ"]').should('have.css', 'position', 'sticky')
    
[Cypress check element position](https://stackoverflow.com/questions/49823998/cypress-check-element-position)
