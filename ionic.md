# IONIC

- Project set up
- Ionic components
- Routing
- Slot
- Custom Event
- Ionic alert controller
- Modal
- Spinner
- Action Sheet Controller
  
## Project set up

### Ionic install

`npm install -g @ionic/cli`
`ionic start`
-> Select the required options

### Run the app

`ionic serve`
Open the browser, usually at \*\*localhost:8100\*\*

### Generate pages, components, services with Angular

`ionic generate page | component | service [nameOfModule]`

## Ionic components

`<ion-app><ion-app/>` should wrap the entire html page or the SPA if using Angular for example.

```javascript
<ion-header>
<ion-toolbar color="primary">
<ion-buttons slot="start">
<ion-back-button defaultHref="/path">
<ion-title>
<ion-buttons slot="primary">
<ion-button >
<ion-icon name="name" slot="icon-only">

<ion-content [fullscreen]="true">
<ion-grid>
<ion-row>
<ion-col size="12">

<ion-list> // lines="none" to hide the bottom line
<ion-item *ngFor="let item of items">
<ion-label position="floating">
<ion-img [src]=""></ion-img> //  Loads images lazily
<ion-thumbnail> // Renders a squared image
<ion-avatar> // Renders a rounded image
<ion-input autocomplete autocorrect>
<ion-text> // just to style some text

<ion-tabs>
<ion-tab-bar slot="bottom || top">
<ion-tab-button tab="[pathName]">

<ion-card>
<ion-card-header>
<ion-card-title>
<ion-card-subtitle>
<ion-card-content>

<ion-item-sliding *ngFor="let offer of loadedOffers" #slidingItem> // inside of an ion-list
<ion-item-options side="start | end">
<ion-item-option color="secondary" (click)="onMethodName(argumentName, slidingItem)"> // in the component: slindingItem: IonItemSliding => slindingItem.close()

<ion-segment (ionChange)="onMethodName($event)" value="all">
<ion-segment-button value="all">Name 1
<ion-segment-button value="bookable">Name 2 // in the component: event: CustomEvent<SegmentChangeEventDetail>

<ion-spinner>
```

## Routing

- Tabs
- Nav Controller
- Side drawer

Routes/pages/links are not destroyed when navigating to a new page like with Angular but they are cached in Ionic.

### Tabs

```javascript
**<ion-tabs>
  <ion-tab-bar slot="bottom">
    <ion-tab-button tab="[pathName]">
      <ion-label>[Tab Name]</ion-label>
      <ion-icon name="search"></ion-icon>
    </ion-tab-button>
    <ion-tab-button tab="[pathName]">
      <ion-label>[Tab Name]</ion-label>
      <ion-icon name="card"></ion-icon>
    </ion-tab-button>
  </ion-tab-bar>
</ion-tabs>**
```

### NavController
```javascript
import { NavController } from '@ionic/angular';

  constructor(private navCtrl: NavController) { }

  onMethodName() {
    this.navCtrl.navigateBack('[url]'); // with animation
    this.navCtrl.navigateForward('[url]'); // with animation
    this.navCtrl.back();
    this.navCtrl.pop(); 
  }
```

### Side drawer
In the app-component.html:
```javascript
<ion-app>
  <ion-menu menuId="primaryMenu" contentId="main">
    <ion-header>
      <ion-toolbar>
        <ion-title>
          [App title]
        </ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content>
      <ion-list>
        <ion-item lines="none">
          <ion-icon name="business" slot="start"></ion-icon>
          <ion-label>Discover Places</ion-label>
        </ion-item>
        <ion-item lines="none">
          <ion-icon name="checkbox-outline" slot="start"></ion-icon>
          <ion-label>Your Bookings</ion-label>
        </ion-item>
        <ion-item lines="none">
          <ion-icon name="exit-outline" slot="start"></ion-icon>
          <ion-label>Logout</ion-label>
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-menu>
  <ion-router-outlet id="main"></ion-router-outlet>
</ion-app>
```

On the page that should display the menu:
```javascript
<ion-header>
  <ion-toolbar>
    <ion-buttons slot="start">
      <ion-menu-button id="primaryMenu"></ion-menu-button>
    </ion-buttons>
    <ion-title>[pageTitle]</ion-title>
  </ion-toolbar>
</ion-header>
```

Open the menu programatically:
```javascript
import {MenuController} from '@angular/ionic';

  constructor(private menuCtrl: MenuController) { }

  onMethodName() {
    this.menuCtrl.toggle();
    this.menuCtrl.open();
    this.menuCtrl.close();
    this.menuCtrl.isActive();
    this.menuCtrl.isOpen();
  }
```

## Slot

Specific to Web component. Will reserve a space for content to be displayed in the desired position: start, end ...

### Sliding

```javascript
<ion-item-sliding *ngFor="let offer of offers" #slidingItem>
  <ion-item
    [routerLink]="['/', 'places', 'tabs', 'offers', offer.id]"
  >
    <ion-thumbnail slot="start">
      <ion-img [src]="offer.imageUrl"></ion-img>
    </ion-thumbnail>
    <ion-label>
      <h1>{{offer.title}}</h1>
    </ion-label>
  </ion-item>
  <ion-item-options side="start">
    <ion-item-option
      color="secondary"
      (click)="onEdit(offer.id, slidingItem)"
    >
      <ion-icon name="create" slot="icon-only"></ion-icon>
    </ion-item-option>
  </ion-item-options>
  <ion-item-options>
    <ion-item-option
      color="tertiary"
      (click)="onDelete(offer.id)"
    >
      <ion-icon name="create" slot="icon-only"></ion-icon>
    </ion-item-option>
  </ion-item-options>
</ion-item-sliding>
```

## Custom Event

Generic web event:
`public onFilter(event: CustomEvent){}`

## Ionic alert controller

Will create an alert modal

```javascript
import { AlertController } from '@ionic/angular';
  
onMethodName(param: type): returnedType {
  this.alertCtrl.create({
    header: string,
    message: string,
    buttons: [
      {
        text: string,
        role: 'cancel' // will close the modal
      },
      {
        text: string,
        handler: () => {
          // code to run when the button is clicked
        }
      }
    ]
  })
    .then(alertEl => {
      alertEl.present();
    });
}
```

## Modal
Create a component that will act as the content of the modal.
Import the component in the **entryComponent**  array in the module that will present the modal.
Then in the page/component that calls the modal:

```javascript
import {ModalController} from '@angular/ionic';

  onMethodName() {
    this.modalCtrl.create({
      keyboardClose: boolean,
      component: [componentModalName],
      componentProps: { nameOfThePropertyToPass: propertyValue },
      id: 'anyStringName'
    })
      .then(modalEl => {
        modalEl.present();
        return modalEl.onDidDismiss();
      })
      .then(resultData => {
        console.log(resultData.data, resultData.role);
      });
  }
```

In the modal template component:
```javascript
    this.modalCtrl.dismiss({ // first argument is the data, second is the role
      message: whateverValue
    }, 'roleName');
```

## Action Sheet Controller

```javascript
private actionSheetCtrl: ActionSheetController

this.actionSheetCtrl.create({
  header: 'Choose an action',
  buttons: [
    {
      text: 'text 1',
      handler: () => {
        this.methodName();
      }
    },
    {
      text: 'text 2',
      handler: () => {
        this.methodName();
      }
    },
    {
      text: 'Cancel',
      role: 'cancel'
    }
  ]
}).then(actionSheetEl => {
  actionSheetEl.present();
});
```