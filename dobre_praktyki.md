Dobieranie elementów html

1) Sugerowane rozwiązanie jako najlepsze -> dodanie atrybutu `data-cy` stworzonego z myślą o tworzeniu testu jako docelowego selektora - ten rodzaj atrubutu nie jest sparowany z zachowaniem (JS) ani wyglądem strony/przeglądanego elementu (css)

2) Na 2. miejscu sugerowane jest odwoływanie się do wizualnych odnośników np.  cy.contains('Submit').click()

3) Zakres testu nie powinien wychodzić po za testowaną stronę (np. strona płatności)


Źródła

[Best Practices](https://docs.cypress.io/guides/references/best-practices)

