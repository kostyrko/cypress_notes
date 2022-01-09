   
        it.only('should Edit profile data using select input and be in dto', () => {
          cy.intercept('PUT', '/profile', { fixture: 'profile/edit_profile_put_dep.json' }).as('profileDepPut')
          cy.navigateToEditProfile(viewport)
          cy.fixture(fixtureDep).then(data => {
            let body: string
            cy.getEditProfileService()
              .should('have.property', 'profile')
              .then(profile => {
                body = data.dict.body_types[Math.floor(Math.random() * data.dict.body_types.length)]
                // eslint-disable-next-line no-param-reassign
                profile.body_type = body
              })
            // cy.windowTick()
            cy.clickSaveChangesButton()
            cy.get('@profileDepPut').should(value => {
              // @ts-ignore
              // eslint-disable-next-line security/detect-object-injection
              expect(value.request.body.profile).to.have.property('body_type').and.deep.equal(body)
            })
          })
          // cy.getEditProfileService()
          //   .should('have.property', 'profile')
          //   .then(profile => {
          //     cy.log(profile)
          //     profile.body_type = 'Fit'
          //     profile.eye_color = 'Green'
          //   })
          // cy.windowTick()
          // if (isMobile(viewport)) {
          //   profileNavigation.setNavAppAct('Bio')
          // }
          cy.checkPopover()
        })
        
### Źródła

[gleb bahmutov -> testing-angular-application-via-app-actions](https://glebbahmutov.com/blog/testing-angular-application-via-app-actions/)
