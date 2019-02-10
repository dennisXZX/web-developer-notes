## styled-components

#### Simply the function interpolation syntax using partial 

```
// before
const Button = styled.button`
  background-color: ${({ theme }) => theme.bgColor}
  color: ${({ theme }) => theme.textColor}
`

// after
const fromTheme = prop => ({ theme }) => theme[prop]
const getBackgroundColor = fromTheme("bgColor")
const getTextColor = fromTheme("textColor")

const Button = styled.button`
  background-color: ${getBackgroundColor}
  color: ${getTextColor}
`
```

#### Fundamentals

Define a styled component

```js
/* create a Wrapper component that'll render a <section> tag with some styles */
const Wrapper = styled.section`
  padding: 4em;
  /* Adapt the colors based on 'primary' prop */
  background: ${props => props.primary ? "palevioletred" : "papayawhip"};
  color: ${props => props.primary || "palevioletred"};
  
  /* pseudo selectors */
  &:hover {
    color: ${props => props.themeHoverColor};
  }
  
  /* pseudo elements */  
  ::before {
    content: '🚀';
  }  
`;

/* inherit the Wrapper styled component */
const PinkWrapper = styled(Wrapper)`
  color: pink;
`
```

Use styled component

```js
render(
  <Wrapper 
    primary 
    themeColor="blue" 
    themeHoverColor="pink"
  >
    <Title>
      Hello World!
    </Title>
  </Wrapper>
);
```

Render a button component as a link

```js
const Button = styled.button`
  display: inline-block;
  color: palevioletred;
  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid palevioletred;
  border-radius: 3px;
`;

/* use the 'as' polymorphic prop to render an <a> HTML element  */
<Button as="a" href="/">Link with Button styles</Button>
```

#### Multiple conditions for a CSS property in styled component

```js
const Button = styled.button`
  color: ‘white’;
  ${props => props.isSecondary && css`
     color: ‘blue’;
  `}
  ${props => props.isDisabled && css`
     color: ‘grey’;
  `}
`

// Using the disabled iteration of the styled component
<Button isDisabled />
```

The button is styled to have white text by default, but if an ‘isDisabled’ prop is applied, the color property will be overwritten since it appears later in the style declaration, and the button will be given a colour of grey.

