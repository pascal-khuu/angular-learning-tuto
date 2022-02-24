# angular-tuto

<!-- [Edit on StackBlitz ⚡️](https://stackblitz.com/edit/angular-8rdrkj) -->

Partie 1 (auteur:Pascal KHUU)

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

La commande 'ng generate component product-alerts' permet de créer un composant product-alerts avec 4 fichiers
product-alerts.component.html (template de la structure), product-alerts.component.css (mise en forme), product-alerts.component.spec.ts (tests unitaires), product-alerts.component.ts (logique métier).

 Dans le fichier product.alerts.component.ts

```ts
@Component({
  selector: 'app-product-alerts',
  templateUrl: './product-alerts.component.html',
  styleUrls: ['./product-alerts.component.css']
}) 
```

L'annotation @Component permet de créer un composant  avec comme selecteur permettant d'identifier le composant 'app-product-alerts'.
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

 <!-- <button type="button" (click)="share()">Share</button> --> permet de créer un bouton de clic associé à l'évènement share().

 Le composant ProductAlertComponent a come parent le composant ProductListComponent.
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
  Dans l'annotation @NgModule, les différents composants sont déclarés


 dans le fichier product-alert.component.ts
 
 ```ts
 import { Component, Input, Output, EventEmitter } from '@angular/core';
import { Product } from '../products';
 export class ProductAlertsComponent {
  @Input() product: Product | undefined;
  @Output() notify = new EventEmitter();
} 
```
Dans le fichier product-alerts.component.ts,import { Component, Input, Output, EventEmitter } from '@angular/core' permet d'importer un composant, mettre en entrée des données, d'exporter et d'émettre un évènement.
Dans ce fichier, La classe productAlertsComponent est exportée, notify permet d'emettre un évènement lorsque notify change et @Input()product: Product | undefined permet d'importer des données de la classe Produit et ce qui n'est pas défini.

Dans le fichier product-alerts.component.ts

```ts
export class ProductAlertsComponent {
  @Input() product: Product | undefined;
  @Output() notify = new EventEmitter();
  
}
```

la classe ProductAlertsComponent importe des données du produit et exporte notify qui émet un évènement

```html
<p *ngIf="product && product.price > 700">
  <button type="button" (click)="notify.emit()">Notify Me</button>
</p>
```

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
lorsque je clique sur le bouton Notify me , un évènement est lancé (notify.emit()).

```html
 <app-product-alerts
  [product]="product" 
  (notify)="onNotify()">
</app-product-alerts> 
```
app-product-alerts permet d'afficher dans le template html le contenu du message 'You will be notified when the product goes on sale'.


--------------------------------------------------------------------------------------------
Partie 2

La commande 'ng generate component product-details' permet de créer un composant product-details avec les 4 fichiers product-details.component.html(structure du template), product-details.component.css(mise en forme), product-details.component.spec.ts(tests unitaires) et product-details.component.ts(logique métier).

```ts
RouterModule.forRoot([
      { path: '', component: ProductListComponent },
      { path: 'products/:productId', component: ProductDetailsComponent },
```
RouterModule spécifie les différents path pour les composants ProductListComponent et ProductDetailsComponent.
