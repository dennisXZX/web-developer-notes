### React核心技术与开发实战笔记 (王红元)

#### Performance

- Using `style={{ display: 'none' }}` would perform better than `shouldComponentDisplay && Component`, because the latter would update the DOM while the former only change CSS.

- Never use `index` as `key` value as this would make the `diff` algorithm useless.

- Always use `memo()` on functional components to reduce unnecessary renders.

#### setState()

- By default, setState() is asynchronous, it is designed in this way. If setState() is synchronous, it would result in many render() calls, this would reduce performance. Another issue would be in between setState() and render() calls, state and props would be different if setState() is synchronous.

- To make setState() synchronous, you can put it in `setTimeout()` or in native event handler.

#### How React update DOM

- state or props get updated
- render() is re-executed
- a new virtual DOM is created
- compare old virtual DOM and new one
- patch the difference to DOM
