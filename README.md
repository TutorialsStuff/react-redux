# ReactJS Redux Tutorials

## Installations

Create an application:

```
create-react-app reduxexample
```

Run application:

```
cd reduxexample
npm start
```

Install redux:

```
npm i redux react-redux redux-thunk
```

## Components

Create a folder in a root `components`. Create form and js files inside components folder.

## Render example of posts from JSON example

### Component

React Component template (js):

```jsx
import React, { Component } from 'react'

export default class componentName extends {
    render() {
        return (
            <div>
                
            </div>
        )
    }
}
```

Separate to export:

```jsx
import React, { Component } from 'react'

class Posts extends Component {
    render() {
        return (
            <div>
                <h1>Posts</h1>
            </div>
        )
    }
}

export default Posts;
```

Import into App.js:

```js
import Posts from './components/Posts'
```

Into render add:

```js
<Posts />
```

Fetching a JSON dummy data once on reload and dump into console:

```js
componentWillMount() {
        //console.log(123);
        fetch('http://jsonplaceholder.typicode.com/posts')
            .then(res => res.json())
            .then(data => console.log(data));
    }
```

Put into const postItems to iterate posts and set states:

```js
constructor(props) {
        super(props);
        this.state = {
            posts: [ ]
        }
    }

    componentWillMount() {
        //console.log(123);
        fetch('http://jsonplaceholder.typicode.com/posts')
            .then(res => res.json())
            .then(data => this.setState({ posts: data }));
    }
```

Map a data into render():

```js
    const postItems = this.state.posts.map();
```

Mapping a data into const and render using postItems:

```js
    render() {
        const postItems = this.state.posts.map(post => (
            <div key={post.id}>
                <h3>{post.title}</h3>
                <p>{post.body}</p>
            </div>
        ));

        return (
            <div>
                <h1>Posts</h1>
                {postItems}
            </div>
        )
    }
```

## Create a post

Make a new file inside components/Postform.js with PostForm class:

```js
import React, { Component } from 'react'

class PostForm extends Component {
    constructor(props) {
        super(props);
        this.state = {}
    }

    render() {
        return (
            <div>
                <h1>Add Post</h1>
            </div>
        )
    }
}

export default PostForm;
```

Insert into App.js to render form:

```js
import React, { Component } from 'react';
import './App.css';

import Posts from './components/Posts'
import PostForm from './components/Postform'

class App extends Component {
  render() {
    return (
      <div className="App">
        <PostForm/>
        <hr />
        <Posts />
      </div>
    );
  }
}

export default App;
```

Create a form to render input fields (Title and Body):

```js
render() {
        return (
            <div>
                <h1>Add Post</h1>
                <form>
                    <div>
                        <label>Title: </label><br />
                        <input name="title" type="text" />
                    </div>
                    <br />
                    <div>
                        <label>Body: </label><br />
                        <input name="body" />
                    </div>
                    <br />
                    <button type="submit">Submit</button>
                </form>
            </div>
        )
    }
```

Make event onChange based on values:

```js
constructor(props) {
        super(props);
        this.state = {
            title: '',
            body: ''
        }

        this.onChange = this.onChange.bind(this);
    }

    onChange(e) {
        this.setState({ [e.target.name]: e.target.value })
    }
    
```

Set up to make events on change in fields and default values:

For text input:

```js
    <input
        name="title"
        type="text"
        value={this.state.title}
        onChange={this.onChange}
    />
```

and for body text:

```js
    <input
        name="body"
        value={this.state.body}
        onChange={this.onChange}
    />
```

That will be visible on React state and changes.

Make a form on submit to send a data on render:

```js
 <form onSubmit={this.onSubmit}>
```

Fetch a data on submit:

```js
constructor(props) {
        super(props);
        this.state = {
            title: '',
            body: ''
        }
        
        this.onChange = this.onChange.bind(this);
        this.onSubmit = this.onSubmit.bind(this);
    }

    onChange(e) {
        this.setState({ [e.target.name]: e.target.value })
    }

    onSubmit(e) {
        e.preventDefault();

        const post = {
            title: this.state.title,
            body: this.state.body
        };

        fetch('http://jsonplaceholder.typicode.com/posts', {
            method: 'POST',
            headers: {
                'content-type': 'application/json'
            },
            body: JSON.stringify(post)
        })
            .then(res => res.json())
            .then(data => console.log(data));
    }
```

# Redux (reducers, store)

After install of redux packages, import a package into App.js:

Add a Provider, redux imports, const store and <Provider> component:

```js
import React, { Component } from 'react';
import './App.css';
import { Provider } from 'react-redux';
import { createStore, applyMiddleware } from 'redux';

import Posts from './components/Posts'
import PostForm from './components/Postform'

const store = createStore(() => [], {}, applyMiddleware());

class App extends Component {
  render() {
    return (
        <Provider store={store}>
          <div className="App">
            <PostForm/>
            <hr />
            <Posts />
          </div>
        </Provider>
    );
  }
}

export default App;
```

Create a file store.js in a root.

Import it in App:

```jsx
import store from './store';
```

Put into store.js to create initial reducer:

```jsx
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';

const initialState = {};

const middleware = [thunk];

const store = createStore(
    rootReducer,
    initialState,
    applyMiddleware(...middleware)
);

export default store;
```

Create a folder reducers in a root.

Inside reducers/ folder create index.js.

Then import into store.js a folder of reducers:

```
import rootReducer from './reducers';
```

Create a index.js into reducers/folder:

```
import { combineReducers } from 'redux';
import postReducer from './postReducer';

export default combineReducers({
    posts: postReducer
});
```

Create a postReducer.js file inside reducers.

* Actions

Create a folder actions on a root of project.
Create types.js file inside actions folder.

```
export const FETCH_POSTS = 'FETCH_POSTS';
export const NEW_POSTS = 'NEW_POSTS';
```

In postReducer.js put:

```
import { FETCH_POSTS, NEW_POSTS } from "../actions/types";

const initialState = {
    items: [],
    item: {}
}

export default function(state = initialState, action) {
    switch (action.type) {
        case FETCH_POSTS:
            console.log('reducer');
            return {
                ...state,
                items: action.payload
            };
        default:
            return state;

    }
}

```

Remove from Posts.js components:

```js
    constructor(props) {
        super(props);
        this.state = {
            posts: [ ]
        }
    }

    componentWillMount() {
        //console.log(123);
        fetch('http://jsonplaceholder.typicode.com/posts')
            .then(res => res.json())
            .then(data => this.setState({ posts: data }));
    }
```

On actions folder create postActions.js:

```
import { FETCH_POSTS, NEW_POSTS } from "../actions/types";

export const fetchPosts = () => dispatch => {
    console.log('fetching');
    fetch('http://jsonplaceholder.typicode.com/posts')
        .then(res => res.json())
        .then(posts => dispatch({
            type: FETCH_POSTS,
            payload: posts
        }));
}
```

Make active reducers inside reducers/index.js:

```
const mapStateToProps = state => ({
    posts: state.posts.items
});

export default connect(null, { fetchPosts })(Posts);
```

On a Posts.js insert proptypes:

import PropTypes from 'prop-types';

```
Posts.propTypes = {
    fetchPosts: PropTypes.func.isRequired,
    posts: PropTypes.array.isRequired
}
```
    
### References

* [JSON Placeholder](https://jsonplaceholder.typicode.com/)
* [JSON Placeholder posts](http://jsonplaceholder.typicode.com/posts)
