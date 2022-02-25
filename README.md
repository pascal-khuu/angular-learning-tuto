# angular-tuto

<!-- [Edit on StackBlitz ⚡️](https://stackblitz.com/edit/angular-8rdrkj) -->

## Partie 1 Getting started (auteur:Pascal KHUU)

app-product-list est le contenu où s'affichent les téléphones ainsi que leur description avec les boutons Share et Notify.
Dans app, se trouve tous les composants du projet.
Dans app.module.ts se trouve les dépendances du projet (modules du projet).
product-alerts et product-list sont des composants.

```html
<div *ngFor="let product of products">
  <h3>
  <a [title]="product.name + ' details'">
      {{ product.name }}
    </a>
  </h3>
</div>
```

*ngFor="let product of products" permet d'itérer dans la liste des produits dans le div avec la valeur du nom du produit (' {{ product.name }}')en les affichant dans la page web ("Phone XL", "Phone Mini", "Phone Standard").

```html
 <a [title]="product.name + ' details'"> 
 ``` 
permet d'afficher le nom puis details lorsque je
je pointe avec la souris sur le nom d'un produit comme "Phone XL details" .

```html
 <p *ngIf="product.description">
    Description: {{ product.description }}
  </p> 
```
*ngIf"=product.description" permet s'il y a une description des téléphones d'afficher la description de chaque téléphone.

La commande 'ng generate component product-alerts' permet de créer un composant product-alerts avec 4 fichiers: product-alerts.component.html (template de la structure), product-alerts.component.css (mise en forme), product-alerts.component.spec.ts (tests unitaires), product-alerts.component.ts (logique métier).

 Dans le fichier product.alerts.component.ts

```ts
@Component({
  selector: 'app-product-alerts',
  templateUrl: './product-alerts.component.html',
  styleUrls: ['./product-alerts.component.css']
}) 
```

L'annotation @Component permet de créer un composant  avec comme sélecteur permettant d'identifier le composant 'app-product-alerts'.
Le templateUrl est l'url (gestion de la structure du template)  avec comme url './product-alerts.component.html'.
 Le styleUrls (gestion de la mise en forme) a comme url './product-alerts.component.css'.

 ```ts
 import { Component, OnInit, Input } from '@angular/core'
 ```
  permet de créer un composant ('Component'), d'initialiser le composant ('OnInit') et d'importer des données ('Input').

```ts
  export class ProductAlertsComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }
  ``` 
  implémente la classe OnInit qui permet d'initialiser toutes les propriétés liées aux données d'une directive

  ```html
  <button type="button" (click)="share()">Share</button> 
  ```
   permet de créer un bouton de clic associé à l'évènement share().

 Le composant ProductAlertComponent a comme parent le composant ProductListComponent.
```html
  <p *ngIf="product && product.price > 700">
  <button type="button">Notify Me</button>
</p> 
```
Si le produit existe et le prix du produit est supérieur à 700, un bouton Notify apparaît sous le téléphone.

```
@NgModule({
  declarations: [
    AppComponent,
    TopBarComponent,
    ProductListComponent,
    ProductAlertsComponent,
  ],
```
  Dans l'annotation @NgModule, les différents composants sont déclarés.


 Dans le fichier product-alert.component.ts
 
 ```ts
 import { Component, Input, Output, EventEmitter } from '@angular/core';
import { Product } from '../products';
 export class ProductAlertsComponent {
  @Input() product: Product | undefined;
  @Output() notify = new EventEmitter();
} 
```
Dans le fichier product-alerts.component.ts,import { Component, Input, Output, EventEmitter } from '@angular/core' permet d'importer un composant, mettre en entrée des données, d'exporter et d'émettre un évènement.
Dans ce fichier, La classe productAlertsComponent est exportée, notify permet d'émettre un évènement lorsque notify change et @Input()product: Product | undefined permet d'importer des données de la classe Produit et ce qui n'est pas défini.

Dans le fichier product-alerts.component.ts

```ts
export class ProductAlertsComponent {
  @Input() product: Product | undefined;
  @Output() notify = new EventEmitter();
  
}
```

la classe ProductAlertsComponent importe des données du produit et exporte notify qui émet un évènement



 Dans le fichier product-list-component.ts

 ```ts
 export class ProductListComponent {

  products = products;

  share() {
    window.alert('The product has been shared!');
  }

  onNotify() {
    window.alert('You will be notified when the product goes on sale');
  }
} 
```

Avec la méthode share(), lorsque je clique le bouton share, cela affiche dans une fenêtre 'The product has been shared!'.
Avec la méthode share(), lorsque je clique le bouton Notify me, cela affiche dans une fenêtre 'You will be notified when the product goes on sale'.

Dans le composant product-alerts-component qui est l'enfant du composant product-list-component
```html
<p *ngIf="product && product.price > 700">
  <button type="button" (click)="notify.emit()">Notify Me</button>
</p>
```
lorsque je clique sur le bouton Notify me , un évènement est lancé (notify.emit()).

```html
 <app-product-alerts
  [product]="product" 
  (notify)="onNotify()">
</app-product-alerts> 
```
app-product-alerts permet d'afficher dans le template html le contenu du message 'You will be notified when the product goes on sale' lorsque je clique sur le bouton Notify me.


--------------------------------------------------------------------------------------------
## Partie 2 Adding Navigation

La commande 'ng generate component product-details' permet de créer un composant product-details avec les 4 fichiers: product-details.component.html(structure du template), product-details.component.css(mise en forme), product-details.component.spec.ts(tests unitaires) et product-details.component.ts(logique métier).

```ts
RouterModule.forRoot([
      { path: '', component: ProductListComponent },
      { path: 'products/:productId', component: ProductDetailsComponent },
```
RouterModule spécifie les différents paths pour les composants ProductListComponent et ProductDetailsComponent.

```ts
import { ActivatedRoute } from '@angular/router';
```
ActivatedRoute permet d'importer en Angular un router qui permet d'utiliser un service et fournit l'accès aux informations sur un itinéraire qui est associé à un composant.

(fichier product-details.component.ts)
```ts
constructor(private route: ActivatedRoute) { }
```
Lorsqu'on instancie le composant product-details, on le construit avec ActivatedRoute qui contient des informations sur le route et sa configuration.
(fichier product-details.component.ts)
```ts
product: Product | undefined;
```
permet de déclarer le produit qui peut aussi être indéfini.
```ts
const routeParams = this.route.snapshot.paramMap;
  const productIdFromRoute = Number(routeParams.get('productId'));
```
ici, on paramètre le route avec le productId pour afficher les détails de chaque téléphone.

```ts
  this.product = products.find(product => product.id === productIdFromRoute);
}
```
permet de chercher l'id du produit par le route

```ts
<div *ngIf="product">
  <h3>{{ product.name }}</h3>
  <h4>{{ product.price | currency }}</h4>
  <p>{{ product.description }}</p>
</div>
```
grâce au route , lorsque je clique le produit (téléphone), cela affiche ses caractéristiques par l'id grâce au route:
son nom, son prix, sa description.

----------------------------------------------------------------------------------------------
## Partie 3 Managing data
'ng generate service cart' permet de créer deux fichiers cart.service.spec.ts et cart.service.ts.

```ts
export class CartService {
  items: Product[] = [];
/* . . . */

  addToCart(product: Product) {
    this.items.push(product);
  }

  getItems() {
    return this.items;
  }

  clearCart() {
    this.items = [];
    return this.items;
  }
  ```
  est un service qui permet de manipuler des objets produits soit en l'ajoutant (méthode addToCart),
  soit en l'accèdant (méthode getItems) et en le vidant (méthode clearCart).

  Dans le composant product-details (fichier product-details.component.ts)
  ```ts
  import { CartService } from '../cart.service';
  ```
  on importe le service cart.

  ```ts
  export class ProductDetailsComponent implements OnInit {

  constructor(
    private route: ActivatedRoute,
    private cartService: CartService
  ) { }
}
```
Il y a une instanciation par la classe ProductDetailsComponent grâce à son constructeur qui se fait avec deux champs privés route:ActivatedRoute et cartService.
```ts
export class ProductDetailsComponent implements OnInit {

  addToCart(product: Product) {
    this.cartService.addToCart(product);
    window.alert('Your product has been added to the cart!');
  }
}
```
on ajoute la méthode addToCart qui permet d'ajouter une carte d'un produit (téléphone) avec le message d'alerte 'Your product has been added to the cart!'.

```html
<button type="button" (click)="addToCart(product)">Buy</button>
</div>
```
permet d'ajouter un bouton Buy dans la page html du composant product details.
Lorsque je clique sur ce bouton, l'évènement est associé  à la méthode addToCart(product) qui permet d'ajouter le produit à la carte.

ng generate component cart permet de créer un composant cart avec 4 fichiers crées: cart.component.html (une structure de template), cart.component.css (mise en forme), cart.component.ts (logique métier) et  cart.component.spec.ts (tests unitaires).
```ts
@Component({
  selector: 'app-cart',
  templateUrl: './cart.component.html',
  styleUrls: ['./cart.component.css']
})
```
permet d'avoir comme sélecteur 'app-cart', une url de template './cart.component.html' (template structure) et une url de style './cart.component.css' (mise en forme).
```ts
@NgModule({
  declarations: [
    AppComponent,
    TopBarComponent,
    ProductListComponent,
    ProductAlertsComponent,
    ProductDetailsComponent,
    CartComponent,
  ]
  ```
  Le composant CartComponent a été ajouté. </br>
  { path: 'cart', component: CartComponent } a été ajouté dans app.module.ts

  ```ts (top-bar.component.html)
  <a routerLink="/cart" class="button fancy-button"><i class="material-icons">shopping_cart</i>Checkout</a>
  ```
  routerlink permet de lier le contenu du html du composant cart.
  Lorsque je clique sur le bouton checkout, cela affiche le contenu du html du composant cart.

dans cart.component.ts
  ```ts
  import { CartService } from '../cart.service';
  ```
  permet d'importer le contenu du composant CartService.</br>

dans le fichier cart.component.ts
  ```ts
  export class CartComponent {

items = this.cartService.getItems();
  constructor(
    private cartService: CartService
  ) { }
}
```
permet d'instancier le cartService et items accède au cartService.
dans card.component.html
```html
<h3>Cart</h3>

<div class="cart-item" *ngFor="let item of items">
  <span>{{ item.name }}</span>
  <span>{{ item.price | currency }}</span>
</div>
```
Cela affiche les produits et leur prix achetés dans le html du composant card.

Le fichier shipping.json contient les données des téléphones en json (type, price).

HttpClientModule utilise le HTTPClient (flux entre le client et le serveur) en l'important dans cart.service.ts
```ts
import { HttpClientModule } from '@angular/common/http';
```
dans cart.service.ts
```ts
export class CartService {
/* . . . */
  getShippingPrices() {
    return this.http.get<{type: string, price: number}[]>('/assets/shipping.json');
  }
}
```
Ici on accède avec la méthode getShippingPrices avec le get le type et le prix dans le fichier json shipping.json.
La commande 'ng  generate component shipping' génère 4 fichiers du composant: shipping.component.html(structure du template),shipping.component.spec.ts(tests unitaires),shipping.component.ts(logique métier) et shipping.component.css(mise en forme).

dans app.module.ts
```ts
declarations: [
    AppComponent,
    TopBarComponent,
    ProductListComponent,
    ProductAlertsComponent,
    ProductDetailsComponent,
    CartComponent,
    ShippingComponent
  ],
 ```
 Le composant ShippingComponent s'est ajouté dans app.module.ts.

dans shipping.component.ts
 ```ts
 import { CartService } from '../cart.service';
 ```
 cela importe cart-service dans ship.component.ts.

dans app.module.ts
 ```ts
 @NgModule({
  imports: [
    BrowserModule,
    HttpClientModule,
    ReactiveFormsModule,
    RouterModule.forRoot([
      { path: '', component: ProductListComponent },
      { path: 'products/:productId', component: ProductDetailsComponent },
      { path: 'cart', component: CartComponent },
      { path: 'shipping', component: ShippingComponent },
    ])
  ```
  cela importe le chemin de shipping { path: 'shipping', component: ShippingComponent },

 'shippingCosts = this.cartService.getShippingPrices();' permet de récupérer les prix de cartService (prix des téléphones achetés)

```html
 <h3>Shipping Prices</h3>

<div class="shipping-item" *ngFor="let shipping of shippingCosts | async">
  <span>{{ shipping.type }}</span>
  <span>{{ shipping.price | currency }}</span>
</div>
```
Lorsque je clique sur shipping prices, cela affiche un ensemble de nombre de jours  et de prix .

------------------------------------------------------------------------------------------------
## Partie 4 Using forms for user input

Formbuilder permet de réaliser un formulaire de contrôle.

```ts
export class CartComponent {

  constructor(
    private cartService: CartService,
    private formBuilder: FormBuilder,
  ) {}
}
```
On instancie un cartcomponent avec deux champs privés cartService et formBuilder.

```ts
checkoutForm = this.formBuilder.group({
    name: '',
    address: ''
  });
  ```
  Un groupe de formulaire avec le nom et l'adresse est associé au formulaire.
  import { FormBuilder } from '@angular/forms'; permet d'importer formbuilder dans le composant card component.

```ts
onSubmit(): void {
    // Process checkout data here
    this.items = this.cartService.clearCart();
    console.warn('Your order has been submitted', this.checkoutForm.value);
    this.checkoutForm.reset();
  }
```
Lorsque l'évènement OnSubmit() a été lancé, un message est affiché 'Your order has been submitted' avec le nom et l'adresse saisis  du formulaire dans la console.
La page cart est réinitialisée(this.checkoutForm.reset()), tous les téléphones achetés sont vidés


```html
  <form [formGroup]="checkoutForm" (ngSubmit)="onSubmit()">
</form>
```
ngsubmit() permet d'associer à l'évènement onSubmit() en Angular au formulaire de groupe "checkoutForm" qui correspond à l'évènement onSubmit() qui permet de vider les téléphones payés et d'afficher dans la console 'Your order has been submitted' avec le nom et l'adresse saisis  du formulaire.

```html
 <form [formGroup]="checkoutForm" (ngSubmit)="onSubmit()">

  <div>
    <label for="name">
      Name
    </label>
    <input id="name" type="text" formControlName="name">
  </div>

  <div>
    <label for="address">
      Address
    </label>
    <input id="address" type="text" formControlName="address">
  </div>

  <button class="button" type="submit">Purchase</button>

</form>
```
est un formulaire avec les champs name et adresse avec le bouton purchase de type submit associé à l'évènement onSubmit() qui vide les téléphones achetés et affiche sur la console le message 'Your order has been submitted' avec le nom et l'adresse saisis du formulaire.


