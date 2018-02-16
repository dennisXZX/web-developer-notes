## Ionic

#### Ionic CLI

`ionic start projectName templateName` to create a new project.

`ionic serve` to launch the app in the browser. (run this command in the project folder)

`ionic run android` to launch the app in your device which should be connected with USB.

#### Navigation

Navigation in Ionic works like a stack data structure. You `push` to a new page and `pop` back to a previous page.

```ts
// navigate to the ItemDetailsPage
itemTapped(event, item) {
  // passing item to the ItemDetailsPage
  this.navCtrl.push(ItemDetailsPage, item);
}

// in ItemDetailsPage, we can use NavParams to accept the navigation parameter
constructor(public navCtrl: NavController, public navParams: NavParams) {
  this.team = this.navParams.data;
}
```

If we need to pass a navigation parameter to a tab, we need to use `[rootParams]`
