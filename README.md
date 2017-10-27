# React LifeCycle Methods and their usages.
React provides a series of hooks we can tap into at each phase of the life cycle. These method hooks inform us of where the Component is in the life cycle and what we can and cannot do.Each of the life cycle methods are called in a specific order and at a specific time. The methods are also tied to different parts of the life cycle. Here are the methods broken down in order and by their corresponding life cycle phase
# Mounting
These methods are called when an instance of a component is being created and inserted into the DOM:

constructor()

componentWillMount()

render()

componentDidMount()

# Updating
An update can be caused by changes to props or state. These methods are called when a component is being re-rendered:

componentWillReceiveProps()

shouldComponentUpdate()

componentWillUpdate()

render()

componentDidUpdate()

# Unmounting
This method is called when a component is being removed from the DOM:

componentWillUnmount()
# Error Handling
This method is called when there is an error during rendering, in a lifecycle method, or in the constructor of any child component.

componentDidCatch()
# State change or Prop Changes :
whenever the state or Prop change a React component will automatically render the new view. this is important becuase all you your components should have the updated view. so when ever you call this.setState() or props change there is a set of lifecycle hooks that will be called again to  make sure your view gets updated. 

1.shouldComponentUpdate(nextProps,nextState)

2.componentWillUpdate(nextProps,nextState)

3.render()

4.componentDidUpdate(prevProps,prevState)


# 1.ren​der()
The render() method is required.
The render() function should be pure, meaning that it does not modify component state, it returns the same result each time it’s invoked, and it does not directly interact with the browser. If you need to interact with the browser, perform your work in componentDidMount() or the other lifecycle methods instead. Keeping render() pure makes components easier to think about.
##### Note
render() will not be invoked if shouldComponentUpdate() returns false.
You can also return multiple items from render() using an array:
Don’t forget to add keys to elements in a fragment to avoid the key warning.[incase of repeating elements like lists]

# 2.constructor()
The constructor for a React component is called before it is mounted. When implementing the constructor for a React.Component subclass, you should call super(props) before any other statement. Otherwise, this.props will be undefined in the constructor, which can lead to bugs.
Avoid introducing any side-effects or subscriptions in the constructor. For those use cases, use componentDidMount() instead.
The constructor is the right place to initialize state. To do so, just assign an object to this.state; don’t try to call setState() from the constructor. The constructor is also often used to bind event handlers to the class instance.
If you don’t initialize state and you don’t bind methods, you don’t need to implement a constructor for your React component.
In rare cases, it’s okay to initialize state based on props. This effectively “forks” the props and sets the state with the initial props. Here’s an example of a valid React.Component subclass constructor.

`constructor(props) {
  super(props);
  this.state = {
    color: props.initialColor
  };}
  `

If you “fork” props by using them for state, you might also want to implement componentWillReceiveProps(nextProps) to keep the state up-to-date with them. But lifting state up is often easier and less bug-prone.


# 3.componentWillMount()
componentWillMount() is invoked immediately before mounting occurs. It is called before render(), therefore calling setState() synchronously in this method will not trigger an extra rendering. Generally, we recommend using the constructor() instead.
Avoid introducing any side-effects or subscriptions in this method. For those use cases, use componentDidMount() instead.

# 4.componentDidMount
componentDidMount() is invoked immediately after a component is mounted. Initialization that requires DOM nodes should go here. If you need to load data from a remote endpoint, this is a good place to instantiate the network request.
This method is a good place to set up any subscriptions. If you do that, don’t forget to unsubscribe in componentWillUnmount().
Calling setState() in this method will trigger an extra rendering, but it is guaranteed to flush during the same tick. This guarantees that even though the render() will be called twice in this case, the user won’t see the intermediate state. 
Use this pattern with caution because it often causes performance issues. It can, however, be necessary for cases like modals and tooltips when you need to measure a DOM node before rendering something that depends on its size or position.

# 5.componentWillReceiveProps(nextProps)

​this lifecycle will give you access to nextProps as well as the older existing props that are already available for the component

componentWillReceiveProps() is invoked before a mounted component receives new props. If you need to update the state in response to prop changes (for example, to reset it), you may compare this.props and nextProps and perform state transitions using this.setState() in this method.
Calling this.setState() within this function will not Trigger an additional re-render, and we can access the old props via this.props

Note that React may call this method even if the props have not changed, so make sure to compare the current and next values if you only want to handle changes. This may occur when the parent component causes your component to re-render.
React doesn’t call componentWillReceiveProps() with initial props during mounting. It only calls this method if some of component’s props may update. Calling this.setState()generally doesn’t trigger componentWillReceiveProps().
# 6.shouldComponentUpdate(nextProps,nextState)
Use shouldComponentUpdate() to let React know if a component’s output is not affected by the current change in state or props. The default behavior is to re-render on every state change, and in the vast majority of cases you should rely on the default behavior.
if you want to reduce unnecessary render you can compare nextprops,nextState with this.props and this.state and decide if you want to execute render(). return false to stop the next render.
shouldComponentUpdate() is invoked before rendering when new props or state are being received. Defaults to true. This method is not called for the initial render or when forceUpdate() is used.
Returning false does not prevent child components from re-rendering when their state changes.
Currently, if shouldComponentUpdate() returns false, then componentWillUpdate(), render(), and componentDidUpdate() will not be invoked. 
Note that in the future React may treat shouldComponentUpdate() as a hint rather than a strict directive, and returning falsemay still result in a re-rendering of the component.
If you determine a specific component is slow after profiling, you may change it to inherit from React.PureComponent which implements shouldComponentUpdate() with a shallow prop and state comparison. If you are confident you want to write it by hand, you may compare this.props with nextProps and this.state with nextState and return false to tell React the update can be skipped.
You SHOULD NOT do deep equality checks or using JSON.stringify() in shouldComponentUpdate(). It is very inefficient and will harm performance.

# 7.componentWillUpdate(nextProps,nextState)

componentWillUpdate() is invoked immediately before rendering when new props or state are being received. Use this as an opportunity to perform preparation before an update occurs. This method is not called for the initial render.
Note that you cannot call this.setState() here; nor should you do anything else (e.g. dispatch a Redux action) that would trigger an update to a React component before componentWillUpdate() returns.
If you need to update state in response to props changes, use componentWillReceiveProps() instead.

# 8.componentDidUpdate(prevProps,PrevState)

componentDidUpdate() is invoked immediately after updating occurs. This method is not called for the initial render.
Use this as an opportunity to operate on the DOM when the component has been updated. This is also a good place to do network requests as long as you compare the current props to previous props (e.g. a network request may not be necessary if the props have not changed).

componentDidUpdate() will not be invoked if shouldComponentUpdate() returns false.
# 9.componentWillUnmount()
componentWillUnmount() is invoked immediately before a component is unmounted and destroyed. Perform any necessary cleanup in this method, such as invalidating timers, canceling network requests, or cleaning up any subscriptions that were created in componentDidMount().
# 10.componentDidCatch(error,info)
Error boundaries are React components that catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI instead of the component tree that crashed. Error boundaries catch errors during rendering, in lifecycle methods, and in constructors of the whole tree below them.
A class component becomes an error boundary if it defines this lifecycle method. Calling setState() in it lets you capture an unhandled JavaScript error in the below tree and display a fallback UI. Only use error boundaries for recovering from unexpected exceptions; don’t try to use them for control flow.

### Note
Error boundaries only catch errors in the components below them in the tree. An error boundary can’t catch an error within itself.


appart from these lifecycle methods there are other 2 more methods which execute only once in the lifetime of the react component. these are : 

getDefaultProps() and

getDefaultState()
​




