hej, w naszym projekcie w automatach uwierzytelniamy sie do aplikacji niewygasającym tokenem, ale dostalismy info, że musimy to zmienic ze względów bezpieczeństwa; wygląda na to że, bedziemy musieli logowac sie przez strone logowania. Kazdy nasz test zaczyna sie od logowania (a jest ich duuuzo) i sadze ze to bardzo wydluzy czas trwania testów; czy znacie moze jakies alternatywne roziwazania, które zastosowaliscie u siebie w projektach?

Można się przez API logować i korzystać ze zwykłych tokenów zamiast niewygasających


logowanie przez API 
lub
użycie cy.session() https://docs.cypress.io/api/commands/session - logujesz się w pierwszym teście, a później cypress 'trzyma' sesję
Cypress DocumentationCypress Documentation
session | Cypress Documentation
Cache and restore cookies, localStorage, and sessionStorage in order to reduce test setup times. Experimental The session API is currently experimental,
