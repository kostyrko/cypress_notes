test all options // test all select options

    it.only('selected option should match the one chosen one in Physical, Lifestyle and Basic Section', () => {
              cy.navigateToEditProfile(viewport)
              profileNavigation.clickMobileLifestyle()
              cy.get('.form-group > .select-wrap > [data-cy]')
                .getAttributes('data-cy')
                .then(ids => {
                  const labelsArr = ids.filter(function (value, index, arr): boolean {
                    return index % 2 === 0
                  })
                  const selectArr = ids.filter(item => !labelsArr.includes(item))
                  selectArr.forEach((select, i) => {
                    cy.get(`[data-cy="${select}"]`)
                      .find('option')
                      .then(options => {
                        const actual: {}[] = [...options].map(obj => obj.childNodes[0]['data'])
                        actual.forEach(option => {
                          cy.get(`[data-cy="${select}"]`).select(`${option}`)
                          cy.get(`[data-cy="${labelsArr[i]}"]`).should('contain', `${option}`)
                        })
                      })
                  })
                })
            })
    
get attributes (used above)
    
    Cypress.Commands.add(
      'getAttributes',
      {
        prevSubject: true,
      },
      (subject, attr) => {
        const attrList: string[] = []
        cy.wrap(subject).each($el => {
          cy.wrap($el)
            .invoke('attr', attr)
            .then(attr => attrList.push(attr))
        })
        return cy.wrap(attrList)
      },
    )
