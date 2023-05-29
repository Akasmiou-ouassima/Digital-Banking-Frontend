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

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {
}
```
## Les interfaces

_**Page Home**_

