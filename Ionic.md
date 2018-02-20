## Ionic

#### Ionic CLI

`ionic start projectName templateName` to create a new project.

`ionic serve` to launch the app in the browser. (run this command in the project folder)

`ionic run android` to launch the app in your device which should be connected with USB.

#### Pages vs components

In Ionic, pages are used as a container for components. Normally we first structure our app using pages, then we can create individual components and place them into a page. A page does not need a selector because it is loaded dynamically by Ionic, however, keeping the selector property is handy as you can use it to target page-level style.

In each `.scss` file named after the `.html` one, you would see we use the page/component selector to target the specific page/component.

```
page-hello-ionic {
  // page level or component level style
}
```

#### Navigation

Navigation in Ionic works like a stack data structure. You `push` to a new page and `pop` back to a previous page.

```ts
// navigate to the ItemDetailsPage
itemTapped(event, item) {
  // passing item to the ItemDetailsPage, the 'ItemDetailsPage' is a string because this page is lazily loaded
  this.navCtrl.push('ItemDetailsPage', {
    item: item
  });
}

// in ItemDetailsPage, we can use NavParams to accept the navigation parameter in the constructor
constructor(public navCtrl: NavController, public navParams: NavParams) {
  this.team = navParams.get('item');
}
```

If we need to pass a navigation parameter to a tab, we need to use `[rootParams]`

You can use `this.navCtrl.popToRoot()` to navigate back to the root page of the root navigation stack. However, in Ionic, tab controls has its own navigation stack. You need to use `this.nav.parent.parent.popToRoot()` to navigate back to the root page of the root navigation stack from within a tab page.

#### Life cycle events

`ionViewDidLoad` runs when the page loaded, __it only runs once__.

`ionViewDidEnter` runs when you enter a page successfully.

`ionViewDidLeave` runs when you leave a page successfully.

`ionViewDidUnload` runs when a page is popped out of the navigation stack.
