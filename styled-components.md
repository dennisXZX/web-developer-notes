## styled-components

#### Fundamentals

Define a styled component

```js
/* Create a Wrapper component that'll render a <section> tag with some styles */

const Wrapper = styled.section`
  padding: 4em;
  /* Adapt the colors based on primary prop */
  background: ${props => props.primary ? "palevioletred" : "white"};
`;

// A new component based on Wrapper, but with some override styles
const TomatoWrapper = styled(Wrapper)`
  color: tomato;
  border-color: tomato;
`;
```

Use styled component

```js
render(
  <Wrapper primary>
    <Title>
      Hello World!
    </Title>
  </Wrapper>
);
```
