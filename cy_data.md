### dynamiczne nadawanie cy.data w Angularze

    <div class="profile-edit-nav">
      <div
        class="nav-item"
        *ngFor="let nav of editSvc.mobileNav"
        [ngClass]="editSvc.getNavClass(nav)"
        (click)="editSvc.setCurrentNav(nav)"
      >
        <span [attr.data-cy]="'nav' + nav">{{ nav }}</span>
      </div>
    </div>
    
    
   ### Źródło
   
   [Using angular variables inside non-angular attributes](https://stackoverflow.com/questions/22621735/using-angular-variables-inside-non-angular-attributes)
