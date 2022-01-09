
#### Wrap

metod **wrap** przetrzymuje przekazany do niej obiekt (jeśli jest to promise to wówczas przechowuje jego wynik) 


#### Invoke

metoda **invoke** wywołuje przekazaną funkcję lub metodę



### Its

pozwala na pozyskanie wartości przypisanej do atrybutu wywołanego/przetrzymanego elementu

    cy.title().its('length').should('eq', 24)


    const fn = () => {
      return 42
    }

    cy.wrap({ getNum: fn }).its('getNum').should('be.a', 'function')

Źródła:

[wrap - cypress.io](https://docs.cypress.io/api/commands/wrap#Syntax)

[invoke - cypress.io](https://docs.cypress.io/api/commands/invoke#Function)

[its - cypress.io](https://docs.cypress.io/api/commands/its#Syntax)
