### React Testing Library

#### See all roles in a component

```js
screen.getByRole('');
```

#### getBy vs queryBy vs findBy

TL;DR - For any element that isn't there yet but will be there eventually, use findBy over getBy or queryBy. If you assert for a missing element, use queryBy. Otherwise default to getBy.

- getBy returns an element or an error.

```js
// This won't work, getBy throws an error before we can make the assertion
expect(screen.getByText(/Searches for JavaScript/)).toBeNull();
```

- queryBy does not return an error when you query an element that isn't there.

```js
// This will work
expect(screen.queryByText(/Searches for JavaScript/)).toBeNull();
```

- findBy is used for asynchronous elements which will be there eventually.

```js
  test('renders App component', async () => {
    render(<App />);
 
    expect(screen.queryByText(/Signed in as/)).toBeNull();
 
    expect(await screen.findByText(/Signed in as/)).toBeInTheDocument();
  });
```

After its initial render, we assert that the "Signed in as" text is not there by using the queryBy instead of the getBy search variant. Then we await the new element to be found, and it will be found eventually when the promise resolves and the component re-renders again.

