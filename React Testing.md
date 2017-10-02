## React Testing

### General Test Structure

```
// use 'describe' to group together similar tests
describe('ComponentName', () => {
  // use 'it' to test a single attribute of a target
  it('shows the correct text', () => {
    // use 'expect' to make an assertion about a target
    expect(component).to.contain('React simple starter');
  });
});
```

### Testing Assertions

```
describe('CommentBox', () => {
  let component;

  beforeEach(() => {
    // initialize the component
    component = renderComponent(CommentBox);
  });

  // check if a component contains another component
  it('shows a comment box', () => {
    expect(component.find('.comment-box')).to.exist;
  });  

  // check if a component has the correct CSS class
  it('has the correct class', () => {
    expect(component).to.have.class('comment-box');
  });

  // check if a component is correctly rendered
  it('has a text area', () => {
    expect('', () => {
      expect(component.find('textarea')).to.exist;
    });
  });

  // check if a component contains the correct text
  it('shows the correct text', () => {
    expect(component).to.contain('React simple starter');  
  }); 

  // this group is about events
  describe('entering some text into the comment box', () => {
    beforeEach(() => {
      // simulate a change event on the textarea
      component.find('textarea').simulate('change', 'new comment');
    });

    it('shows that text in the textarea', () => {
      // check if the textarea has the correct value after the simulated change event
      expect(component.find('textarea')).to.have.value('new comment');
    });

    it('when submitted, clears the input', () => {
      // simulate a submit event
      component.simulate('submit');
      // check if it the textarea has the correct value after the simulated submit event
      expect(component.find('textarea')).to.have.value('');
    });
  });

});
```
