# Digital-Banking-Frontend

<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Project/blob/main/Les%20images/icon.png" align="right" />

 ## 🔗  Digital-Banking-Project


>**Le travail à effectuer consiste à développer une application Web basée sur Spring et Angular pour la gestion de comptes bancaires.**
 
 **Les technologies suivantes seront utilisées dans ce dépôt :**
- [x] Frontend : Angular

## Installation de Angular
``` 
npm install -g @angular/cli
```

## Création d'un nouveau projet
``` 
ng new Digital-Banking-Frontend
```
## Exécuter le projet
```
ng serve
```
## Structure de projet

<img src="structure.jpg" align="right" />

##  les composants Angular pour l'interface utilisateur
<img src="composants.jpg" align="right" />

## Models 

>Les modèles servent à représenter et à structurer les données dans une application.
_**Account**_
```java
export interface AccountDetails {
  accountId:               string;
  balance:                 number;
  currentPage:             number;
  totalPages:              number;
  pageSize:                number;
  accountOperationDTOList: AccountOperation[];
}

export interface AccountOperation {
  id:              number;
  operationDate:   Date;
  amount:          number;
  operationType:   string;
  transactionType: string;
  description:     string;
}
```
_**Customer**_
```java
export interface Customer {
  id: number;
  name: string;
  email: string;
}
```
_**Customer Account**_
```java
export interface Account {
  type:          string;
  id:            string;
  balance:       number;
  createdDate:   Date;
  status:        string;
  customerDTO:   Customer;
  overDraft?:    number;
  interestRate?: number;
}
```
## Services 
>**Les services jouent le rôle de fournir des fonctionnalités et des méthodes réutilisables pour la manipulation des données et la logique métier dans une application.**
>**Pour communiquer avec le backend dans le module "customers", vous devez injecter HttpClient en utilisant le constructeur. HttpClient est une classe fournie par Angular qui permet d'effectuer des requêtes HTTP vers un serveur. 
>En l'injectant dans le module "customers", vous pourrez l'utiliser pour envoyer des requêtes HTTP et recevoir des réponses du backend.**

_**Customer**_

```java
@Injectable({
  providedIn: 'root'
})
export class AccountsService {

  constructor(private http: HttpClient) { }

  public getAccountOperations(accountId: string, page: number, size: number): Observable<AccountDetails> {
    return this.http.get<AccountDetails>(environment.backendHost+"/accounts/" + accountId + "/pageOperations?page=" + page + "&size=" + size);
  }

  public debit(accountId: string, amount: number, description: string) {
    let data = {accountId: accountId, amount: amount, description: description}
    return this.http.post(environment.backendHost+"/accounts/debit", data);
  }

  public credit(accountId: string, amount: number, description: string) {
    let data = {accountId: accountId, amount: amount, description: description}
    return this.http.post<AccountDetails>(environment.backendHost+"/accounts/credit", data);
  }

  public transfer(accountDestination:string,accountSource:string, amount: number, description: string) {
    let data = {accountDestination,accountSource,amount,description:description}
    return this.http.post(environment.backendHost+"/accounts/transfer", data);
  }

  public newCurrentAccount(balance: number, overDraft: number, customerId: number) {
    let data = {balance, overDraft, customerId}
    return this.http.post(environment.backendHost+"/customers/" + customerId +"/current-accounts?overDraft=" + overDraft + "&initialBalance=" + balance , data);
  }
  public newSavingAccount(balance: number, interestRate: number, customerId: number) {
    let data = {balance, interestRate, customerId}
    return this.http.post(environment.backendHost+"/customers/" + customerId +"/saving-accounts?interestRate=" + interestRate + "&initialBalance=" + balance , data);
  }
}
```
_**Account**_

```java
@Injectable({
  providedIn: 'root'
})
export class AccountsService {

  constructor(private http: HttpClient) { }

  public getAccountOperations(accountId: string, page: number, size: number): Observable<AccountDetails> {
    return this.http.get<AccountDetails>(environment.backendHost+"/accounts/" + accountId + "/pageOperations?page=" + page + "&size=" + size);
  }

  public debit(accountId: string, amount: number, description: string) {
    let data = {accountId: accountId, amount: amount, description: description}
    return this.http.post(environment.backendHost+"/accounts/debit", data);
  }

  public credit(accountId: string, amount: number, description: string) {
    let data = {accountId: accountId, amount: amount, description: description}
    return this.http.post<AccountDetails>(environment.backendHost+"/accounts/credit", data);
  }

  public transfer(accountDestination:string,accountSource:string, amount: number, description: string) {
    let data = {accountDestination,accountSource,amount,description:description}
    return this.http.post(environment.backendHost+"/accounts/transfer", data);
  }

  public newCurrentAccount(balance: number, overDraft: number, customerId: number) {
    let data = {balance, overDraft, customerId}
    return this.http.post(environment.backendHost+"/customers/" + customerId +"/current-accounts?overDraft=" + overDraft + "&initialBalance=" + balance , data);
  }
  public newSavingAccount(balance: number, interestRate: number, customerId: number) {
    let data = {balance, interestRate, customerId}
    return this.http.post(environment.backendHost+"/customers/" + customerId +"/saving-accounts?interestRate=" + interestRate + "&initialBalance=" + balance , data);
  }
}
```
## Routes
>**Les routes permettent de définir les chemins et les actions correspondantes dans une application web.**

```java
const routes: Routes = [
  {path: '', redirectTo: 'home', pathMatch: 'full'},
  {
    path: 'home',
    component: HomeComponent,
  },
  {
    path: 'login',
    component: LoginPageComponent,
    canActivate: [AuthGuard]
  },
  {
    path: "customers", component: CustomersComponent,
    canActivate: [MainGuard],
    canLoad: [MainGuard]
  },
  {
    path: "accounts", component: AccountsComponent, canActivate: [MainGuard],
    canLoad: [MainGuard]
  },
  {
    path: "accounts/:id", component: AccountsComponent, canActivate: [MainGuard],
    canLoad: [MainGuard]
  },
  {
    path: "new-customer", component: NewCustomerComponent, canActivate: [MainGuard],
    canLoad: [MainGuard]
  },
  {
    path: "update-customer/:id", component: UpdateCustomerComponent, canActivate: [MainGuard],
    canLoad: [MainGuard]
  },
  {
    path: "customer-accounts/:id", component: CustomerAccountsComponent, canActivate: [MainGuard],
    canLoad: [MainGuard]
  },
  {
    path: "bank-accounts", component: BankAccountsComponent, canActivate: [MainGuard],
    canLoad: [MainGuard]
  },
  {
    path: "customers/new-account/:id", component: NewAccountComponent, canActivate: [MainGuard],
    canLoad: [MainGuard]
  },
  {
    path: "edit-profil/:id", component: EditProfilComponent, canActivate: [MainGuard],
    canLoad: [MainGuard]
  },
  {
    path: "about", component: AboutComponent,
  },
  {
    path: "account-operations/:id", component: AccountOperationsComponent,canActivate: [MainGuard],
    canLoad: [MainGuard]
  },

];
```
```java
@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {
}
```
## Les interfaces

> 

### _Page Home_

<div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/pagehome.jpg" />
 </div>

 <div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/redirectLogin.jpg" />
 </div>
 
 ### _Page Login_
 
  <div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/login1.jpg"  />
 </div>

 
_**Ici l'authentification est effectuée en utilisant un compte d'administrateur**_  

 <div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/login.jpg"  />
 </div>
 
 _**En cas d'erreur dans les informations de connexion, une notification d'erreur sera généralement affichée pour informer l'utilisateur que les informations fournies sont incorrectes**_  
 
 <div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/erreurInfoLogin.jpg"  />
 </div>
 
 ### _Liste des clients_
                                                                                                            
<div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/customers.jpg" />
 </div>
  
 ### _Chercher un Client_
 
 <div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/search.jpg">
 </div> 
 
  ### _Chercher un Client_
  
 <div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/search.jpg">
 </div>                                                                                                           

  ### _Modifier un Client_
  
<div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/update-customer.jpg">
</div>

<div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/updateCustomerAlert.jpg">
</div>

<div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/updateCustomerAlert1.jpg">
</div>

   ### _Ajouter un Client_
   
<div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/add-newCustomer.jpg">
</div>

   ### _Supprimer un Client_
   
<div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/deleteCustomer.jpg">
</div>

<div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/deleteCustomer1.jpg">
</div>

  ### _Afficher les comptes d'un Client_
  
<div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/customers1.jpg">
</div>

<div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/AccountsCustomer.jpg">
</div>

 ### _Afficher les operations d'un Compte_
 
<div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/operationsIndex.jpg">
</div>

<div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/accountOperations.jpg">
</div>

 ### _Ajouter un Compte_
 
 <div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/addAccount.jpg">
</div>

 <div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/addAccountAlert.jpg">
</div>

 ### _Afficher tous les Comptes_
 
  <div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/allAccounts.jpg">
</div>

 ### _Chercher un Compte_
 
   <div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/earchAccount.jpg">
</div>

_**Si le compte n'existe pas, une notification d'erreur sera généralement affichée**_  

<div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/AccountNotFound.jpg">
</div>

_**Maintenant, nous accédons en utilisant un compte utilisateur**_

 ### _les Comptes d'utilisateur_
 
 <div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/userAccounts.jpg">
</div>

 ### _les operations du Compte d'utilisateur_
 
 <div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/userAccountOperations.jpg">
</div>

 ### _Modifier les informations d'utilisateur_
 
 _**Ici, l'utilisateur peut modifier ses informations**_  
 
 <div align="center">
<img src="https://github.com/Akasmiou-ouassima/Digital-Banking-Frontend/blob/main/les%20images/EditProfileUser.jpg">
</div>


 
