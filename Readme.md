Subject:

Redux is a **state management framework** that can be used with a number of different web technologies, including React.

In Redux, there is a **single state object** that's responsible for the entire state of your application. 

This means if you had a React app with ten components, and each component had its own local state, the entire state of your app would be defined by a **single state object** housed in the **Redux store**.

Any time any piece of your app wants to update state, it **must** do so through the Redux store. 

The **unidirectional data flow** makes it easier to track state management in your app.

The Redux store is an object which **holds** and **manages** application state. 

There is a method called **createStore()** on the Redux object, which you use to create the Redux store

This method takes a **reducer function** as a required argument


Example Code:
```
    const reducer = (state = 5) => {
        return state;
    }
    const store = Redux.createStore(reducer);
```


You can retrieve the current state held in the Redux store object with the **getState()** method.

Example Code:
```
    const store = Redux.createStore(
        (state = 5) => state
    );
    const currentState = store.getState();
```


In Redux, all state updates are triggered by **dispatching actions** 

An **action** is simply a **JavaScript object** that contains information about an **action event** that has occurred. 

The Redux store receives these action objects, then updates its state accordingly. 

Sometimes a Redux action also carries some **data**. 

For example, the action carries a username after a user logs in. 

While the data is optional, actions must carry a **type property** that specifies the 'type' of action that occurred.


Think of Redux actions as **messengers** that deliver information about **events** happening in your app to the Redux store. 

The store then conducts the business of updating state based on the action that occurred.

Writing a **Redux action** is as simple as declaring an object with a type property

Example Code: Define an action 
```
    const action = {
        type: 'LOGIN'
    }
```


After creating an action, the next step is sending the action to the Redux store so it can update its state

In Redux, you define **action creators** to accomplish this 

An action creator is simply a **JavaScript function** that returns an action

**Action creators** create objects that represent action events

Example Code: 
```
    const action = {
        type: 'LOGIN'
    }
    const actionCreator = () => {
        return action;
    };
```


**dispatch method** is what you use to dispatch actions to the Redux store

Calling **store.dispatch()** and passing the value returned from an **action creator** sends an action back to the store

The following lines are equivalent, and both dispatch the action of type LOGIN

Example Code: 
```
    store.dispatch(actionCreator());
    store.dispatch({ type: 'LOGIN' });
```

The Redux store has an initialized state that's an object containing a **login property** currently set to false

Example Code: 
```
    const store = Redux.createStore(
        (state = {login: false}) => state
    );
    const loginAction = () => {
        return {
            type: 'LOGIN'
        }
    };

    store.dispatch(loginAction());
```


After an action is created and dispatched, the Redux store needs to know how to respond to that action. 

This is the job of a **reducer function**

**Reducers** in Redux are responsible for the state modifications that take place in response to actions. 

A **reducer** takes **state** and **action** as arguments, and it always returns a **new state**

Another key principle in Redux is that **state is read-only** 

In other words, the reducer function must always **return a new copy** of state and **never modify state directly**

Redux does not enforce state immutability, however, you are responsible for enforcing it in the code of your reducer functions

Example Code: 
```
    const defaultState = {
        login: false
    };
    const reducer = (state = defaultState, action) => {
        if(action.type === "LOGIN"){
            return state = {login: true};
        } else {
            return state;
        }
    };
    const store = Redux.createStore(reducer);
    const loginAction = () => {
        return {
            type: 'LOGIN'
        }
    };
```


You can tell the Redux store how to handle **multiple action types**

Say you are managing **user authentication** in your Redux store. 

You want to have a state representation for when users are **logged in** and when they are **logged out**

You represent this with a single state object with the property **authenticated**. 

You also need **action creators** that create actions corresponding to user login and user logout, along with the **action objects** themselves

Example Code: 
```
    const defaultState = {
        authenticated: false
    };

    const authReducer = (state = defaultState, action) => {
        switch (action.type){
            case 'LOGIN':
                return {authenticated: true};
            break;
            case 'LOGOUT':
                return {authenticated: false};
            break;
            default:
                return state;
        }
    };

    const store = Redux.createStore(authReducer);

    const loginUser = () => {
    return {
        type: 'LOGIN'
    }
    };

    const logoutUser = () => {
    return {
        type: 'LOGOUT'
    }
    };
```


A common practice when working with Redux is to assign **action types** as **read-only constants**, then reference these constants wherever they are used

Example Code: 
```
    const LOGIN = 'LOGIN';
    const LOGOUT = 'LOGOUT';

    const defaultState = {
        authenticated: false
    };

    const authReducer = (state = defaultState, action) => {
        switch (action.type) {
            case LOGIN: 
                return {
                    authenticated: true
                }
            case LOGOUT: 
                return {
                    authenticated: false
                }

            default:
                return state;
        }
    };

    const store = Redux.createStore(authReducer);

    const loginUser = () => {
        return {
            type: LOGIN
        }
    };

    const logoutUser = () => {
        return {
            type: LOGOUT
        }
    };
```


Another method you have access to on the Redux store object is **store.subscribe()**

This allows you to **subscribe listener functions** to the store, which are called whenever an action is dispatched against the store

One simple use for this method is to subscribe a function to your store that simply logs a message every time an action is received and the store is updated

Example Code: 
```
    const ADD = 'ADD';
    const reducer = (state = 0, action) => {
        switch(action.type) {
            case ADD:
            return state + 1;
            default:
            return state;
        }
    };
    const store = Redux.createStore(reducer);
    
    let count = 0;
    store.subscribe(()=>count++);

    store.dispatch({type: ADD});
    console.log(count);

    store.dispatch({type: ADD});
    console.log(count);

    store.dispatch({type: ADD});
    console.log(count);
```


When the state of your app begins to grow more complex, it may be tempting to divide state into multiple pieces. 

Instead, remember the first principle of Redux: all app state is held in a **single state object** in the store. 

Therefore, Redux provides **reducer composition** as a solution for a complex state model. 

You define **multiple reducers** to handle different pieces of your application's state, then compose these reducers together into **one root reducer**. 

The root reducer is then passed into the Redux **createStore()** method.


In order to let us combine multiple reducers together, Redux provides the **combineReducers()** method. 

This method accepts an object as an argument in which you define properties which **associate keys to specific reducer functions**. 

The name you give to the **keys** will be used by Redux as the name for the associated piece of **state**.

Typically, it is a good practice to create a reducer for each piece of application state when they are distinct or unique in some way. 


For example, in a **note-taking app** with user authentication, one reducer could handle authentication while another handles the text and notes that the user is submitting. 

For such an application, we might write the **combineReducers()** method like this

Example Code: 
```
    const rootReducer = Redux.combineReducers({
        auth: authenticationReducer,
        notes: notesReducer
    });
```

Now, the key **notes** will contain all of the state associated with our notes and handled by our **notesReducer**. 

In this example, the state held in the Redux store would then be a single object containing **auth** and **notes** properties

Example Code: 
```
    const INCREMENT = 'INCREMENT';
    const DECREMENT = 'DECREMENT';
    const counterReducer = (state = 0, action) => {
        switch(action.type) {
            case INCREMENT:
            return state + 1;
            case DECREMENT:
            return state - 1;
            default:
            return state;
        }
    };

    const LOGIN = 'LOGIN';
    const LOGOUT = 'LOGOUT';
    const authReducer = (state = {authenticated: false}, action) => {
        switch(action.type) {
            case LOGIN:
            return {
                authenticated: true
            }
            case LOGOUT:
            return {
                authenticated: false
            }
            default:
            return state;
        }
    };

    const rootReducer = Redux.combineReducers({
        auth: authReducer,
        count: counterReducer
    })

    const store = Redux.createStore(rootReducer);
```


You can also **send specific data** along with your actions. 

In fact, this is very common because actions usually originate from some user interaction and tend to carry some data with them

Example Code: 
```
    const ADD_NOTE = 'ADD_NOTE';
    const notesReducer = (state = 'Initial State', action) => {
        switch(action.type) {
            case ADD_NOTE:
            return action.text
            default:
            return state;
        }
    };
    const addNoteText = (note) => {
        return {
            type: ADD_NOTE,
            text: note
        }
    };
    const store = Redux.createStore(notesReducer);
    console.log(store.getState());
    
    store.dispatch(addNoteText('Hello!'));
    console.log(store.getState());
```


asynchronous actions, Redux provides middleware designed specifically for this purpose, called **Redux Thunk middleware**

To include Redux Thunk middleware, you pass it as an argument to **Redux.applyMiddleware()**

This statement is then provided as a second optional parameter to the **createStore()** function

Example Code: 
```
    const store = Redux.createStore(
        asyncDataReducer,
        Redux.applyMiddleware(ReduxThunk.default)
    );
```

To create an **asynchronous action**, you return a **function** in the action creator that takes **dispatch** as an argument

Example Code: 
```
    const handleAsync = () => {
        return function(dispatch) {

    }}
```

Within this function, you can **dispatch actions** and perform **asynchronous requests**

It's common to **dispatch** an action before initiating any asynchronous behavior so that your application state knows that some data is being requested 
(this state could display a loading icon, for instance)

Then, once you receive the data, you dispatch another action which carries the data as a payload along with information that the action is completed

Example Code: 
```
    const REQUESTING_DATA = 'REQUESTING_DATA'
    const RECEIVED_DATA = 'RECEIVED_DATA'

    const requestingData = () => { return {type: REQUESTING_DATA} }

    const receivedData = (data) => { return {type: RECEIVED_DATA, users: data.users} }

    const handleAsync = () => {
        return function(dispatch) {
            store.dispatch(requestingData());
            console.log(store.getState());
            
            setTimeout(function() {
                let data = {
                    users: ['Jeff', 'William', 'Alice']
                }
                store.dispatch(receivedData(data));
                console.log(store.getState());
            }, 2500);
        }
    };

    const defaultState = {
        fetching: false,
        users: []
    };

    const asyncDataReducer = (state = defaultState, action) => {
        switch(action.type) {
            case REQUESTING_DATA:
            return {
                fetching: true,
                users: []
            }
            case RECEIVED_DATA:
            return {
                fetching: false,
                users: action.users
            }
            default:
            return state;
        }
    };

    const store = Redux.createStore(
        asyncDataReducer,
        Redux.applyMiddleware(ReduxThunk.default)
    );

    store.dispatch(handleAsync());
```


Step 13 - Write a Counter with Redux







