## emotion

#### Fundamentals

```js
import { css } from 'emotion'

const color = 'orange'

// create class name with styles
const textClass = css`
  color: hotpink;
  
  &:hover {
    color: ${color};  // <-- dynamic styling
  }  
`

// use it in React component
<div className={textClass}>
  Some hotpink text.
</div>
```

Define a styled component

```js
/* Create a Wrapper component that'll render a <section> tag with some styles */

const Wrapper = styled('section')`
  padding: 4em;
  /* Adapt the colors based on primary prop */
  background: ${props => props.primary ? "palevioletred" : "papayawhip"};
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

#### Composition

