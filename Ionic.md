## Ionic

#### Ionic CLI

`ionic start projectName templateName` to create a new project.

`ionic serve` to launch the app in the browser.

#### Navigation

Navigation in Ionic works like a stack data structure.

```ts
itemTapped(event, item) {
  this.navCtrl.push(ItemDetailsPage, {
    item: item
  });
}
```
