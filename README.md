<img src="https://devmounta.in/img/logowhiteblue.png" width="250" align="right">

# Project Summary

This is a working project that currently only utilizes state and props to pass its data. Before getting started, try to get familiar with the code and see how props is being passed through to different components. In particular, take a look at `src/App.js`. You'll notice that `src/App.js` is currently very large. With the use of `redux`, we'll be able to turn `src/App.js` into a small, 27 line, file. 

During this project, we'll be improving on a web application that walks users through filling out a home loan application. We'll be modifying components that have already been built to use `redux`. This allows us to keep track of data and pass it to the correct components via routing. We'll be making changes to all files in the `src/components` folder, except for `src/components/Finish/Finish.js`, to modify them to work with `redux`. We'll also make changes to `src/router.js`, `src/index.js` and `src/App.js`.

## Setup

* `fork` and `clone` this repository.
* `cd` into the project directory.
* Run `npm i` to install current dependencies.
* Run `npm i react-redux redux react-router-dom`
* Run `npm start` ( The app should intentionally not compile correctly ).

## Step 1

### Summary

In this step, we will create our `store`. 
When using redux, the store holds the entire state of our application. So it's important we set this up first.

## Instructions

* Create a new file in your `src` folder called `store.js`
* Open `src/store.js`.
* Import `createStore` from `redux`.
* Import `reducer.js` from `src/ducks/reducer.js`.
* Export `createStore` by default and pass it reducer as it's argument

<details>

<summary> Detailed Instructions </summary>

<br />

In redux, components need to connect to a store. Let's create this store. Create a file in the `src` folder called `store.js`. We'll only be needing one thing from `redux`: `createStore`. `createStore` will allows us to export the creation of our store. 

```js
import { createStore } from "redux";
```

In order to create our store, we'll also need our reducer. So let's import that as well. Our reducer is located in `src/ducks`.

```js
import { createStore } from "redux";

import reducer from "./ducks/reducer";
```

Now that we have everything we need to import, let's export by default the creation of our store. 

```js
import { createStore } from "redux";

import reducer from "./ducks/reducer";

export default createStore( reducer );
```

</details>

### Solution

<details>

<summary> <code> src/store.js </code> </summary>

```js
import { createStore } from "redux";

import reducer from "./ducks/reducer";

export default createStore( reducer );
```

</details>

## Step 2

### Summary

In this step, we will take our store created from the previous step and hook it up in `src/index.js`. We will also make use of `BrowserRouter` to allow routing in the application. After this step, our app should compile correctly.

### Instructions

* Open `src/index.js`.
* Import `Provider` from `react-redux`.
* Import `store` from `src/store.js`.
* Wrap the `App` component in a `Provider` component:
  * The `Provider` component should have a `store` prop that equals `store` (remember how we call variables in jsx). 
* Import `BrowserRouter` from `react-router-dom`.
* Wrap the `Provider` in `<BrowserRouter>` tags.

<details>

<summary> Detailed Instructions </summary>

<br />

Now that our store is created, we can hook it up to our App in `src/index.js`. This will allow our App to have access to the store and the reducers and will also allow our App to compile correctly. Let's open `src/index.js`. We'll need to import `Provider` from `react-redux` and `store` from `src/store.js`. 

```js
import store from './store'
import { Provider } from 'react-redux'
```

The `Provider` component will "provide" the store to our App. All we need to do is wrap the `App` component in a `Provider` component and give the `Proivder` component a `store` prop that equals `store`. 

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import registerServiceWorker from './registerServiceWorker';
import './index.css';

import store from './store'
import { Provider } from 'react-redux'

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>    
, document.getElementById('root'));
registerServiceWorker();
```

Lastly, since we're using routing we'll be needing to import `BrowserRouter` from `react-router-dom`.

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import registerServiceWorker from './registerServiceWorker';
import './index.css';

import store from './store'
import { Provider } from 'react-redux'
import { BrowserRouter } from 'react-router-dom';

ReactDOM.render(
<BrowserRouter>
    <Provider store={store}>
        <App />
    </Provider>    
</BrowserRouter>
, document.getElementById('root'));
registerServiceWorker();
```

</details>

### Solution

<details>

<summary> <code> src/index.js </code> </summary>

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import registerServiceWorker from './registerServiceWorker';
import './index.css';

import store from './store';
import { Provider } from 'react-redux';
import { BrowserRouter } from 'react-router-dom';

ReactDOM.render(
<BrowserRouter>
    <Provider store={store}>
        <App />
    </Provider>    
</BrowserRouter>
, document.getElementById('root'));
registerServiceWorker();
```
</details>

<img src="https://github.com/DevMountain/home-loan-wizard/blob/solution/readme-assets/1.png" />

## Step 3

### Summary

Because we needed to pass state down to our routes, we had to make our routing a function. Currently, we're exporting a function by default in our `router.js`, by making this a function, we were able to import it into our `App.js` and then pass it props through its arguments. 

In addition to our route component being a function, the way we're connecting our `Route` to the correct component is using a `render={()=> <Component prop={prop}/> path='/'}` instead of a `component={Component} path='/'`. The reason we needed to use render this way was so we could pass props down through the Components element. However, this results in messy looking code.

In this step, we are going to clean up the mess.

## Instructions

* Open `src/router.js`.
* Import `HashRouter` from `react-router-dom`.
    * Instead of using `Switch`, we're going to use `HashRouter` from `react-router-dom` to wrap our routes. 
      * HashRouter uses the hash portion of our URL to keep our UI in sync with the URL when we change views.
* Instead of exporting default a router function, export by default a `HashRouter` component.
  * Now that our routes aren't exported in a function, we don't need to `return` anything.
  * You will start getting errors coming from the props on the routes rendered elements once our routing is no longer a function, this is because they're no longer receiving information via parameters from the function and are now undefined.
* Wrap all the routes in a `<div>`. HashRouter can only have one first level element.
* For each individual route, instead of using render, we will be using component. 
  * A route should look like the following: `<Route component={ theComponent }  path='/thePath'/>`

<br />

Now that we've changed our `router.js`, no data is going to be passed to our other components, you will also get an error that says `TypeError: __webpack_require__.i(...) is not a function`. This is because in our `App.js` router is still being treated as a function.

* Open `src/App.js`.
* Remove the invoking parenthesis from your `{router}` as well as the content inside of them.


<details>

<summary> Detailed Instructions </summary>

<br />

Let's begin by opening `src/router.js` and importing `HashRouter` from `react-router-dom`. We can replace `Switch` with `HashRouter` since we will no longer be using `Switch`.

```js
import { HashRouter, Route } from 'react-router-dom';
```

We can then use this `HashRouter` to reconfigure our router. Let's add an `export default ()` statement above the current export default statement. 

```js
export default (

)
```

In the new export default statement, add a `HashRouter` component that has a `<div>` inside of it. We use this single `<div>` to export all of our routes.

```js
export default (
  <HashRouter>
    <div>

    </div>
  </HashRouter>
)
```

We can then copy and paste all the routes from the old export default statement into your new one. We'll also need to make modifications to each one so that it looks like: `<Route component={ theComponent } path="/thePath" />`. When finished you'll end up with:

```js
export default (
  <HashRouter>
    <div>

      <Route component={NextBtn} exact path= '/'/>
      <Route component={WizardOne}  path='/wOne'/>
      <Route component={WizardTwo}  path="/wTwo"/>
      <Route component={WizardThree} path="/wThree"/>
      <Route component={WizardFour} path="/wFour"/>
      <Route component={WizardFive} path="/wFive"/>
      <Route component={WizardSix} path="/wSix"/>
      <Route component={WizardSeven} path="/wSeven"/>
      <Route component={WizardEight} path="/wEight"/>
      <Route component={WizardNine} path="/wNine"/>
      <Route component={WizardTen} path="/wTen"/>
      <Route component={WizardEleven} path="/wEleven"/>
      <Route component={Finish} path='/finish'/>

    </div>
  </HashRouter>
)
```

Before you exit this file, delete the entire old `export default` statement.

If you noticed the app is no longer working, this is normal. We are restructuring the entire application. We can fix the errors by going into `src/App.js` and modifying how we are using `router`.

```js
  render() {
    return (
      <div>
    
        { router }

      </div>
    );
  }
```

The application should now render correctly again.

</details>

### Solution

<details>

<summary> <code> src/router.js </code> </summary>

```js
import React from 'react';
import WizardOne from './components/WizardOne/WizardOne';
import WizardTwo from './components/WizardTwo/WizardTwo';
import WizardThree from './components/WizardThree/WizardThree';
import WizardFour from './components/WizardFour/WizardFour';
import WizardFive from './components/WizardFive/WizardFive';
import WizardSix from './components/WizardSix/WizardSix';
import WizardSeven from './components/WizardSeven/WizardSeven';
import WizardEight from './components/WizardEight/WizardEight';
import WizardNine from './components/WizardNine/WizardNine';
import WizardTen from './components/WizardTen/WizardTen';
import WizardEleven from './components/WizardEleven/WizardEleven';
import Finish from './components/Finish/Finish';

import NextBtn from './components/NextBtn/NextBtn';
import {  Route, HashRouter } from 'react-router-dom';

export default (
  <HashRouter>
      <div>  

        <Route component={NextBtn} exact path= '/'/>
        <Route component={WizardOne}  path='/wOne'/>
        <Route component={WizardTwo}  path="/wTwo"/>
        <Route component={WizardThree} path="/wThree"/>
        <Route component={WizardFour} path="/wFour"/>
        <Route component={WizardFive} path="/wFive"/>
        <Route component={WizardSix} path="/wSix"/>
        <Route component={WizardSeven} path="/wSeven"/>
        <Route component={WizardEight} path="/wEight"/>
        <Route component={WizardNine} path="/wNine"/>
        <Route component={WizardTen} path="/wTen"/>
        <Route component={WizardEleven} path="/wEleven"/>
        <Route component={Finish} path='/finish'/>

      </div>
  </HashRouter>
)
```
</details>

<details>
<summary><code> src/App.js</code></summary>

```js
import React, { Component } from 'react';
import './App.css';
import router from './router'


class App extends Component {
  constructor(){
    super()
    this.state = {
      loanType: 'Home Purchase',
      propertyType: 'Single Family Home',
      propToBeUsedOn: '',
      city: '',
      found: "false",
      realEstateAgent: "false",
      downPayment: 0,
      cost: 0,
      credit: '',
      history: '',
      addressOne: '',
      addressTwo: '',
      addressThree: '',
      firstName: '',
      lastName: '',
      email: ''
    }
     
    this.handleChangeLoanType = this.handleChangeLoanType.bind(this);
    this.handleChangePropertyType = this.handleChangePropertyType.bind(this);
    this.handleChangePropertyToBeUsedOn = this.handleChangePropertyToBeUsedOn.bind(this);
    this.handleChangeCity = this.handleChangeCity.bind(this);
    this.handleChangeFoundFalse = this.handleChangeFoundFalse.bind(this);
    this.handleChangeFoundTrue = this.handleChangeFoundTrue.bind(this);
    this.handleChangeRealEstateAgentTrue = this.handleChangeRealEstateAgentTrue.bind(this);
    this.handleChangeRealEstateAgentFalse = this.handleChangeRealEstateAgentFalse.bind(this);
    this.handleChangeUpdateDownPayment = this.handleChangeUpdateDownPayment.bind(this);
    this.handleChangeUpdateCost = this.handleChangeUpdateCost.bind(this);
    this.handleChangeCreditE = this.handleChangeCreditE.bind(this);
    this.handleChangeCreditG = this.handleChangeCreditG.bind(this);
    this.handleChangeCreditF = this.handleChangeCreditF.bind(this);
    this.handleChangeCreditP = this.handleChangeCreditP.bind(this);
    this.handleChangeUpdateHistory = this.handleChangeUpdateHistory.bind(this);
    this.handleChangeAddressOne = this.handleChangeAddressOne.bind(this);
    this.handleChangeAddressTwo = this.handleChangeAddressTwo.bind(this);
    this.handleChangeAddressThree = this.handleChangeAddressThree.bind(this);
    this.handleChangeFirstName = this.handleChangeFirstName.bind(this);
    this.handleChangeLastName = this.handleChangeLastName.bind(this);
    this.handleChangeEmail = this.handleChangeEmail.bind(this);
  }
    
    handleChangeLoanType(event) {
        this.setState({loanType : event.target.value});
    }
    handleChangePropertyType(event) {
        this.setState({propertyType : event.target.value});
    }
    handleChangePropertyToBeUsedOn(event){
        this.setState({propToBeUsedOn : event.target.value})
    }
    handleChangeCity(event){
        this.setState({city : event.target.value});
    }
    handleChangeFoundTrue(event){
        this.setState({found : "true"});
    }
    handleChangeFoundFalse(event){
        this.setState({found : "false"});
    }
    handleChangeRealEstateAgentTrue(){
        this.setState({realEstateAgent : "true"});
    }
    handleChangeRealEstateAgentFalse(){
        this.setState({realEstateAgent : "false"});
    }
    handleChangeUpdateDownPayment(event){
        this.setState({downPayment : event.target.value})
    }
    handleChangeUpdateCost(event){
      this.setState({cost: event.target.value});
    }
    handleChangeCreditE(){
      this.setState({credit: 'Excellent'})
    }
    handleChangeCreditG(){
      this.setState({credit: 'Good'})
    }
    handleChangeCreditF(){
      this.setState({credit: 'Fair'})
    }
    handleChangeCreditP(){
      this.setState({credit: 'Poor'})
    }
    handleChangeUpdateHistory(event){
      this.setState({history: event.target.value})
    }
    handleChangeAddressOne(event){
      this.setState({addressOne: event.target.value})
    }
    handleChangeAddressTwo(event){
      this.setState({addressTwo: event.target.value})
    }
    handleChangeAddressThree(event){
      this.setState({addressThree: event.target.value})
    }
    handleChangeFirstName(event){
      this.setState({firstName : event.target.value})
    }
    handleChangeLastName(event){
      this.setState({lastName: event.target.value})
    }
    handleChangeEmail(event){
      this.setState({email: event.target.value})
    }

  render() {
    return (
      <div>
        { router }
      </div>
    );
  }
}

export default App;
```
</details>

## Step 4

### Summary

In this step, we'll be looking at `src/App.js` to modify it to handle state with the `redux` store.

Open `src/App.js`. You'll notice that throughout this file there are comments, these comments are there to more easily show which pieces of state have to do with which views/routes. In this step, we're just going to connect `src/App.js` and the first view to redux. 

Sidenote: You can either leave the constructor function, super, state, all of the bound functions and regular functions that originally dealt with state in there if you'd like. Or it may help to take out the specific ones that deal with state one at a time as you connect things to redux. I'll be removing them all at once. 

## Instructions

* Open `src/App.js`.
* Import `connect` from `react-redux`
* Make a function called `mapStateToProps` just above the `export default` statement.
  * This function should have a parameter called `state`.
  * This function should return the `state` parameter.
* Modify the `export default` statement to use `connect` instead.
  * Be sure to pass in the `mapStateToProps` function.

<details>

<summary> Detailed Instructions </summary>

<br />

Let's begin by opening `src/App.js`. In this file we are going to modify to component to connect to our `redux` store. To do this, we'll need to import `connect` from `react-redux`.

```js
import { connect } from 'react-redux'
```

Before we connect our app to `redux`, we'll need to make a function called `mapStateToProps`. This allows us to choose what parts of state we want from the `redux` store. For now, we'll just return all of the state.

```js
function mapStateToProps( state ) {
  return {
    state
  };
}

export default App;
```

Now that we have a `mapStateToProps` function we can use it with `connect` to connect `src/App.js` to the `redux` store.

```js
function mapStateToProps( state ) {
  return {
    state
  };
}

export default connect( mapStateToProps )( App );
```
</details>

### Solution

<details>

<summary> <code> src/App.js </code> </summary>

```js
import React, { Component } from 'react';
import './App.css';
import router from './router';

import { connect } from "react-redux";


class App extends Component {
  constructor(){
    super()
    this.state = {
      //first view
      loanType: 'Home Purchase',
      propertyType: 'Single Family Home',
      //second view
      city: '',
      //third view
      propToBeUsedOn: '',
      //fourth view
      found: "false",
      //fifth view
      realEstateAgent: "false",
      //sixth view
      downPayment: 0,
      cost: 0,
      //seventh view
      credit: '',
      //eigth view
      history: '',
      //ninth view
      addressOne: '',
      addressTwo: '',
      addressThree: '',
      //tenth view
      firstName: '',
      lastName: '',
      email: ''
    }
    //first view 
    this.handleChangeLoanType = this.handleChangeLoanType.bind(this);
    this.handleChangePropertyType = this.handleChangePropertyType.bind(this);
    //second view
    this.handleChangeCity = this.handleChangeCity.bind(this);
    //third view
    this.handleChangePropertyToBeUsedOn = this.handleChangePropertyToBeUsedOn.bind(this);
    //fourth view
    this.handleChangeFoundFalse = this.handleChangeFoundFalse.bind(this);
    this.handleChangeFoundTrue = this.handleChangeFoundTrue.bind(this);
    //fifth view
    this.handleChangeRealEstateAgentTrue = this.handleChangeRealEstateAgentTrue.bind(this);
    this.handleChangeRealEstateAgentFalse = this.handleChangeRealEstateAgentFalse.bind(this);
    //sixth view
    this.handleChangeUpdateDownPayment = this.handleChangeUpdateDownPayment.bind(this);
    this.handleChangeUpdateCost = this.handleChangeUpdateCost.bind(this);
    //seventh view
    this.handleChangeCreditE = this.handleChangeCreditE.bind(this);
    this.handleChangeCreditG = this.handleChangeCreditG.bind(this);
    this.handleChangeCreditF = this.handleChangeCreditF.bind(this);
    this.handleChangeCreditP = this.handleChangeCreditP.bind(this);
    //eigth view
    this.handleChangeUpdateHistory = this.handleChangeUpdateHistory.bind(this);
    //ninth view
    this.handleChangeAddressOne = this.handleChangeAddressOne.bind(this);
    this.handleChangeAddressTwo = this.handleChangeAddressTwo.bind(this);
    this.handleChangeAddressThree = this.handleChangeAddressThree.bind(this);
    //tenth view
    this.handleChangeFirstName = this.handleChangeFirstName.bind(this);
    this.handleChangeLastName = this.handleChangeLastName.bind(this);
    this.handleChangeEmail = this.handleChangeEmail.bind(this);
  }
    //first view
    handleChangeLoanType(event) {
        this.setState({loanType : event.target.value});
    }
    handleChangePropertyType(event) {
        this.setState({propertyType : event.target.value});
    }
    //second view
    handleChangeCity(event){
        this.setState({city : event.target.value});
    }
    //third view
    handleChangePropertyToBeUsedOn(event){
        this.setState({propToBeUsedOn : event.target.value})
    }
    //fourth view
    handleChangeFoundTrue(event){
        this.setState({found : "true"});
    }
    handleChangeFoundFalse(event){
        this.setState({found : "false"});
    }
    //fifth view
    handleChangeRealEstateAgentTrue(){
        this.setState({realEstateAgent : "true"});
    }
    handleChangeRealEstateAgentFalse(){
        this.setState({realEstateAgent : "false"});
    }
    //sixth view
    handleChangeUpdateDownPayment(event){
        this.setState({downPayment : event.target.value})
    }
    handleChangeUpdateCost(event){
      this.setState({cost: event.target.value});
    }
    //seventh view
    handleChangeCreditE(){
      this.setState({credit: 'Excellent'})
    }
    handleChangeCreditG(){
      this.setState({credit: 'Good'})
    }
    handleChangeCreditF(){
      this.setState({credit: 'Fair'})
    }
    handleChangeCreditP(){
      this.setState({credit: 'Poor'})
    }
    //eigth view
    handleChangeUpdateHistory(event){
      this.setState({history: event.target.value})
    }
    //ninth view
    handleChangeAddressOne(event){
      this.setState({addressOne: event.target.value})
    }
    handleChangeAddressTwo(event){
      this.setState({addressTwo: event.target.value})
    }
    handleChangeAddressThree(event){
      this.setState({addressThree: event.target.value})
    }
    //tenth view
    handleChangeFirstName(event){
      this.setState({firstName : event.target.value})
    }
    handleChangeLastName(event){
      this.setState({lastName: event.target.value})
    }
    handleChangeEmail(event){
      this.setState({email: event.target.value})
    }

  render() {
    return (
      <div>
        { router }
      </div>
    );
  }
}

function mapStateToProps( state ) {
  return {
    state
  };
}

export default connect( mapStateToProps )( App );
```

</details>

## Step 5

### Summary

Now that our `src/App.js` is connected to our `redux` store, let's begin connecting our views. We'll start with the Eleventh view. I chose the Eleventh view because as we start hooking our views up with `redux`, we'll be able to see the data getting to where it needs to end up.

## Instructions 

* Open `src/components/WizardEleven/WizardEleven.js`.
* Import `connect` from `react-redux`
* Connect the component to the `redux store`, just like we did for `src/App.js`.
  * Instead of `returning` the entire state in `mapStateToProps`, return only the properties the component needs:
    * <details>

      <summary> <code> Properties </code> </summary>
      
      ```
      loanType,
      propertyType,
      city,
      propToBeUsedOn,
      found,
      realEstateAgent,
      cost,
      downPayment,
      credit,
      history,
      addressOne,
      addressTwo,
      addressThree,
      firstName,
      lastName,
      email
      ```
      
      </details>
    * You may be scratching your head as to why we are using these exact propeties. When we setup the reducer later on, these will be the names of the properties the `redux` store will be managing. 

<details>

<summary> Detailed Instructions </summary>

<br />

Let's begin by opening `src/components/WizardEleven/WizardEleven.js` and importing `connect` from `react-redux`.

```js
import { connect } from 'react-redux'; 
```

Before we create our `connect` statement, let's make a `mapStateToProps` function just like we did in `src/App.js`. This time, instead of passing the entire state from the store, we are going to pick very specific properties. The list of properties we'll need are listed in the instructions above. 

```js
function mapStateToProps( state ) {
  return{ 
      loanType: state.loanType,
      propertyType: state.propertyType,
      city: state.city,
      propToBeUsedOn: state.propToBeUsedOn,
      found: state.found,
      realEstateAgent: state.realEstateAgent,
      cost: state.cost,
      downPayment: state.downPayment,
      credit: state.credit,
      history: state.history,
      addressOne: state.addressOne,
      addressTwo: state.addressTwo,
      addressThree: state.addressThree,
      firstName: state.firstName,
      lastName: state.lastName,
      email: state.email
  };
}

export default WizardEleven;
```

Now that we have the parts of state we want, we can create our `connect` statement.

```js

function mapStateToProps( state ) {
  return {
    loanType: state.loanType,
    propertyType: state.propertyType,
    city: state.city,
    propToBeUsedOn: state.propToBeUsedOn,
    found: state.found,
    realEstateAgent: state.realEstateAgent,
    cost: state.cost,
    downPayment: state.downPayment,
    credit: state.credit,
    history: state.history,
    addressOne: state.addressOne,
    addressTwo: state.addressTwo,
    addressThree: state.addressThree,
    firstName: state.firstName,
    lastName: state.lastName,
    email: state.email
  };
}

export default connect( mapStateToProps )( WizardEleven ); 
```

</details>

### Solution

<details>

<summary> <code> src/components/WizardEleven/WizardEleven.js </code> </summary>

```js
import React,  { Component } from 'react';
import { connect } from 'react-redux'; 
import { Link } from 'react-router-dom';

class WizardEleven extends Component {
  render(){
    return(
      <div className="parent-div">
        <div className="vert-align">
            <p>Here is an overview of your form:</p> 
            <div >
                <div className="overarching-div">
                    <div className="form" >Name: 
                        <p className="p2">
                            {this.props.firstName} {this.props.lastName}
                        </p>
                    </div> 
                </div>
                <div className="overarching-div">
                    <div className="form" >Email: 
                        <p className="p2">
                          {this.props.email}
                        </p>
                    </div> 
                </div>
                <div className="overarching-div">
                    <div className="form" >What type of loan will you be needing?: 
                        <p className="p2">
                            {this.props.loanType}
                        </p> 
                    </div>
                </div>
                <div className="overarching-div">
                    <div className="form" >What type of property are you purchasing?: 
                        <p className="p2">
                            {this.props.propertyType}
                        </p>
                    </div>
                </div>
                <div className="overarching-div">
                    <div className="form" >In what city will the property be located?: 
                        <p className="p2"> 
                            {this.props.city}
                        </p>
                    </div>
                </div>
                <div className="overarching-div">
                    <div className="form" >Type of property the loan is applied to:
                        <p className="p2">
                            {this.props.propToBeUsedOn}
                        </p> 
                    </div>
                </div>
                <div className="overarching-div">
                    <div className="form" >Have you already found your new home?: 
                        <p className="p2">
                            {this.props.found}
                        </p>
                    </div> 
                </div>
                <div className="overarching-div">
                    <div className="form" >Currently working with a real estate agent?: 
                        <p className="p2">
                            {this.props.realEstateAgent}
                        </p>
                    </div>
                </div>
                <div className="overarching-div">
                    <div className="form" >Estimated purchase price of the home: 
                        <p className="p2">
                            {this.props.cost}
                        </p>
                    </div> 
                </div>
                <div className="overarching-div">
                    <div className="form" >Down payment: 
                        <p className="p2">
                            {this.props.downPayment}
                        </p>
                    </div> 
                </div>
                <div className="overarching-div">
                    <div className="form" >Credit score: 
                        <p className="p2"> 
                            {this.props.credit}
                        </p>
                    </div>
                </div>
                <div className="overarching-div">
                    <div className="form" >Bankruptcy history: 
                        <p className="p2">
                            {this.props.history}
                        </p>
                    </div>
                </div>
                <div className="overarching-div">
                    <div className="form" >Current Address: 
                        <p className="p2">
                            {this.props.addressOne} <br />
                            {this.props.addressTwo} <br />
                            {this.props.addressThree}
                        </p>
                    </div>
                </div>
            </div>   
        </div>
        <div className="row">
            <Link to="/finish"> <button>Send</button></Link>
            <Link to="/"> <button>Start Again</button></Link>
        </div>
      </div>
    )
  }
}

function mapStateToProps( state ) {
  return { 
      loanType: state.loanType,
      propertyType: state.propertyType,
      city: state.city,
      propToBeUsedOn: state.propToBeUsedOn,
      found: state.found,
      realEstateAgent: state.realEstateAgent,
      cost: state.cost,
      downPayment: state.downPayment,
      credit: state.credit,
      history: state.history,
      addressOne: state.addressOne,
      addressTwo: state.addressTwo,
      addressThree: state.addressThree,
      firstName: state.firstName,
      lastName: state.lastName,
      email: state.email
  };
}

export default connect( mapStateToProps )( WizardEleven ); 
```

</details>

## Step 6

### Summary

Now let's take a look at our first view: `src/components/WizardOne/WizardOne.js`. It looks like we need to be able to update the `loanType` and `propertyType` items on state. In redux, in order to update something, we need to have a reducer and action creators.

In this step, we'll start creating our store's reducer and the action creators to update `loanType` and `propertyType`.

## Instructions

* Open `src/ducks/reducer.js`.
* Create an `initialState` object, at the very top of the file, with the following properties:
  * <details>
  
    <summary> <code> Initial State Properties </code> </summary>
    
    ```js
    loanType: 'Home Purchase',
    propertyType: 'Single Family Home',
    city: '',
    propToBeUsedOn: '',
    found: false,
    realEstateAgent: "false",
    cost: 0,
    downPayment: 0,
    credit: '',
    history: '',
    addressOne: '',
    addressTwo: '',
    addressThree: '',
    firstName: '',
    lastName: '',
    email: ''
    ```
    
    </details>
* Modify the `reducer` function to have a state and action parameter. 
  * The `state` parameter should default to `initialState`.
* Create two action types in between `initialState` and the `reducer` function.
  * The first action type should equal: `"UPDATE_LOAN_TYPE"`.
  * The second action type should equal: `"UPDATE_PROPERTY_TYPE"`.
* Create two action creators in between the `reducer` function and the `export default` statement.
  * The first action creator should have a type of `UPDATE_LOAN_TYPE`.
    * This action creator should have a parameter called `loanType`.
    * This action creator should have a payload that equals `loanType`.
  * The second action creator should have a type of `UPDATE_PROPERTY_TYPE`.
    * This action creator should have a parameter called `property`.
    * This action creator should have a payload that equals `property`.
* Modify the `reducer` function to use a `switch` statement on the `action.type`.
  * Create a `case` for `UPDATE_LOAN_TYPE` and update state with the new loan type.
  * Create a `case` for `UPDATE_PROPERTY_TYPE` and update state with the new property type.
  * Create a `default case` that returns state.

<details>

<summary> Detailed Instructions </summary>

<br />

Let's begin by opening `src/ducks/reducer.js`. Our store will need an initial state when it loads for the first time. Let's create a constant variable called `initialState` that will have all of our state's initial values. The property list is listed above in the instructions.

```js
const initialState = {
  loanType: 'Home Purchase',
  propertyType: 'Single Family Home',
  city: '',
  propToBeUsedOn: '',
  found: false,
  realEstateAgent: "false",
  cost: 0,
  downPayment: 0,
  credit: '',
  history: '',
  addressOne: '',
  addressTwo: '',
  addressThree: '',
  firstName: '',
  lastName: '',
  email: ''
};
```

Now that we have an initial state we can update our `reducer` function to have a `state` and `action` parameter. Using `es6` default parameters we can default the `state` parameter to `initialState`. This will happen when the store is first created. 

```js
function reducer( state = initialState, action ) { 

}
```

Before we create the action creators our store needs to determine how to update state, we need to create our action types. We'll put these action types in between our `initialState` and `reducer` function. We'll need two types for this step. One type for updating the loan type and one type for updating the property type. 

```js
const UPDATE_LOAN_TYPE = "UPDATE_LOAN_TYPE";
const UPDATE_PROPERTY_TYPE = "UPDATE_PROPERTY_TYPE";
```

Now that we have our two action types we can create action creators that use them. All an action creator does is return an object with a `type` and `payload` property. The `payload` will equal the value of the parameter in this case. The action creator for loan type will have a `loanType` parameter and the action creator for property type will have a `property` parameter. We also export these action creators so other components can use them. Let's add them under the `reducer` function.

```js
export function updateLoanType( loanType ){
  return {
    type: UPDATE_LOAN_TYPE,
    payload: loanType
  }
}

export function updatePropertyType( property ) {
  return {
    type: UPDATE_PROPERTY_TYPE,
    payload: property
  }
}
```

The last thing we need to do is update the `reducer` function to use a `switch` statement on the `action.type`. This is how our `reducer` will determine which part of state to update. Since we need to keep state `immutable` we will make use of `Object.assign`. Let's start by adding a `switch` statement to our reducer. The `default case` for this switch should just return `state`.

```js
function reducer( state = initialState, action ) { 
  switch( action.type ) {

    default: return state;
  }
}
```

To add our action types to our reducer all we have to do is: `case actionTypeVariableGoesHere`. We'll then use `Object.assign` to get the previous value of state and update its `loanType` or `propertyType` property to the value on the payload.

```js
function reducer( state = initialState, action ) { 
  switch( action.type ) {
    case  UPDATE_LOAN_TYPE:
      return Object.assign( {}, state, { loanType: action.payload } );

    case UPDATE_PROPERTY_TYPE:
      return Object.assign( {}, state, { propertyType: action.payload } );

    default: return state;
  }
}
```
</details>

### Solution

<details>

<summary> <code> src/ducks/reducer.js </code> </summary>

```js
const initialState = {
   loanType: 'Home Purchase',
   propertyType: 'Single Family Home',
   city: '',
   propToBeUsedOn: '',
   found: false,
   realEstateAgent: "false",
   cost: 0,
   downPayment: 0,
   credit: '',
   history: '',
   addressOne: '',
   addressTwo: '',
   addressThree: '',
   firstName: 'aa',
   lastName: '',
   email: ''
}

const UPDATE_LOAN_TYPE = "UPDATE_LOAN_TYPE";
const UPDATE_PROPERTY_TYPE = 'UPDATE_PROPERTY_TYPE'; 

function reducer( state = initialState, action ){ 
    switch( action.type ){
      case UPDATE_LOAN_TYPE:
          return Object.assign( {}, state, { loanType: action.payload } );

      case UPDATE_PROPERTY_TYPE:
          return Object.assign( {}, state, { propertyType: action.payload } );

      default: return state;
    }
} 

export function updateLoanType( loanType ){
  return {
    type: UPDATE_LOAN_TYPE,
    payload: loanType
  }
}

export function updatePropertyType( property ) {
  return {
    type: UPDATE_PROPERTY_TYPE,
    payload: property
  }
}

export default reducer; 
```

</details>

## Step 7

### Summary

In this step, we will connect `src/WizardOne.js` to the store and configure the component to use the action creators we have created so far.

### Instructions

* Import `connect` from `react-redux`.
* Import the update functions you just made from your reducer: `updateLoanType` and `updatePropertyType` from `'./../../ducks/reducer'` (remember to destructure them).
* Currently, in the first select elements `onChange` function, we have `{this.props.handleLoanType}`. This is because we were previously being passed the handle function on props coming from our `App.js`, to the `router.js`, then to our WizardOne component. Now that we're using `redux`, we'll be handling it a little differently.
* Connect the WizardOne component to `redux`, similarly to how we connected our `App.js` to `redux`.
    * Create a `mapStateToProps` function, passing it `state`.
    * Return an object that contains the two pieces of state you'll be updating/wanting access to.
    * The export default will be a little different, this time we'll need to access the destructured functions from our reducer like so: `export default connect(mapStateToProps, {updateLoanType, updatePropertyType})(WizardOne);`
* Now our component is connected to the `redux store`, let's access the function we need to change state. On the first select element on the `onChange` event, set it equal to `{(e) =>this.props.updateLoanType(e.target.value)}`.
    * Because we've connected to `redux`, the updateLoanType function is now on props for this component.
* Go to the second select element's `onChange` even and set it equal to `{(e)=>this.props.updatePropertyType(e.target.value)}`
* Our WizardOne Component should now be hooked up properly and be working with redux! 
* If you'd like to, you can console log what's currently on state by writing `console.log(this.props)` inside of the `render()` function.

<details>

<summary> Detailed Instructions </summary>

* Import `connect` from `react-redux`.

```js
import { connect } from 'react-redux'; 
```

* Import the update functions you just made from your reducer: `updateLoanType` and `updatePropertyType` from `'./../../ducks/reducer'` (remember to destructure them).

```js
import { updateLoanType, updatePropertyType } from './../../ducks/reducer'
```
* Currently, in the first select elements `onChange` function, we have `{this.props.handleLoanType}`. This is because we were previously being passed the handle function on props coming from our `App.js`, to the `router.js`, then to our WizardOne component. Now that we're using `redux`, we'll be handling it a little differently.
* Connect the WizardOne component to `redux`, similarly to how we connected our `App.js` to `redux`.
    * Create a `mapStateToProps` function, passing it `state`.

```js
function mapStateToProps( state ) {

}
```
* Return an object that contains the two pieces of state you'll be updating/wanting access to.

```js
function mapStateToProps( state ) {
  return { 
      loanType: state.loanType,
      propertyType: state.propertyType
    };
}
```
* The export default will be a little different, this time we'll need to access the destructured functions from our reducer like so: `export default connect(mapStateToProps, {updateLoanType, updatePropertyType})(WizardOne);`

```js
export default connect(mapStateToProps, {updateLoanType, updatePropertyType})(WizardOne); 

```
* Now our component is connected to the `redux store`, let's access the function we need to change state. On the first select element on the `onChange` event, set it equal to `{(e) =>this.props.updateLoanType(e.target.value)}`.
    * Because we've connected to `redux`, the updateLoanType function is now on props for this component.

```js
<select onChange={(e) =>this.props.updateLoanType(e.target.value)}>

```
* Go to the second select element's `onChange` even and set it equal to `(e)=>this.props.updatePropertyType(e.target.value)}`

```js
<select onChange={(e)=>this.props.updatePropertyType(e.target.value)}>

```
</details>

### Solution

<details>

<summary> <code> src/components/WizardOne/WizardOne.js </code> </summary>


```js
import React,  { Component } from 'react';
import {Link} from 'react-router-dom';
import { updateLoanType, updatePropertyType } from './../../ducks/reducer'
import { connect } from 'react-redux'; 

class WizardOne extends Component { 

    render(){
        return(
            <div className="parent-div">
                <div className="vert-align">

                <p>What type of loan will you be needing?</p> <br />
            
                <select onChange={(e) =>this.props.updateLoanType(e.target.value)}>

                    <option type="text" value="Home Purchase" >Home Purchase</option>
                    <option type="text" value="Refinance" >Refinance</option>
                    <option type="text" value="Home Equity" >Home Equity loan/line</option>

                </select> <br />  

                <p>What type of property are you purchasing?</p> <br />

                <select onChange={(e)=>this.props.updatePropertyType(e.target.value)}>


                        <option value="Single Family Home">Single Family Home</option>
                        <option value="Town Home">Townhome</option>
                        <option value="Condo">Condo</option>
                        <option value="Multi Family Dwelling">Multi Family Dwelling</option>
                        <option value="Mobile Home">Mobile Home</option>

                </select>
                    
                    <Link to="/wTwo"><button className="margin-btn"> Next </button></Link>
                </div>
            </div>
        )
    }
}
function mapStateToProps( state ) {
  return { 
      loanType: state.loanType,
      propertyType: state.propertyType
    };
}
export default connect(mapStateToProps, {updateLoanType, updatePropertyType})(WizardOne); 
```

</details>


## Step 8

### Summary

Now that we have our first view hooked up. Let's move on to the second View.

## Instructions 

In the `src/ducks/reducer.js`

* Create a new const: `const UPDATE_CITY = 'UPDATE_CITY';`
* Create an action for updating the city:
    * Beneath the reducer function, export a function called `updateCity` and pass it `city` as a parameter.
    * Return an object with a `type` equal to `UPDATE_CITY` and a `payload` equal to `city`.
* Create a reducer to update state for that action. Inside the reducer function:
    * Create a case for `UPDATE_CITY`
    * Return a new object that will become state, pass it empty curly braces, state, and the property with the value that you want to change.


<details>

<summary> Detailed Instructions </summary>

* Create a new const: `const UPDATE_CITY = 'UPDATE_CITY';`
    * We do this because react will throw an error if a variable is misspelled, but not if a string is misspelled.
```js
const UPDATE_CITY = 'UPDATE_CITY';

```

* Create an action for updating the city:
    * All `actions` will return an object with a `type` and `payload`.
    * Beneath the reducer function, export a function called `updateCity` and pass it `city` as a parameter.
    * Return an object with a `type` equal to `UPDATE_CITY` and a `payload` equal to `city`.

```js
export function updateCity(city) {
    return {
        type: UPDATE_CITY,
        payload: city
    }
}
```

* Create a reducer to update state for that action. Inside the reducer function:
    * Create a case for `UPDATE_CITY`
    * Return a new object that will become state, pass it empty curly braces, state, and the property with the value that you want to change.
    * Remember, Object.assign is used to copy values from an original source, the first parameter is curly brackets, showing that we want to make a `new` object, the second parameter is state, which is the object we want to copy all the values of, and the third parameter targets the specific property and it's value that we want to change on this new version of state.

```js
    case UPDATE_CITY:
        return Object.assign({}, state, {city: action.payload})
```
</details>

### Solution

<details>

<summary> <code> src/ducks/reducer.js </code> </summary>

```js
const initialState = {
   loanType: 'Home Purchase',
   propertyType: 'Single Family Home',
   city: '',
   propToBeUsedOn: '',
   found: false,
   realEstateAgent: "false",
   cost: 0,
   downPayment: 0,
   credit: '',
   history: '',
   addressOne: '',
   addressTwo: '',
   addressThree: '',
   firstName: 'aa',
   lastName: '',
   email: ''
}

const UPDATE_LOAN_TYPE = "UPDATE_LOAN_TYPE";
const UPDATE_PROPERTY_TYPE = 'UPDATE_PROPERTY_TYPE';
const UPDATE_CITY = 'UPDATE_CITY';

function reducer(state=initialState, action){ 

    switch(action.type){
        case UPDATE_LOAN_TYPE:
            return Object.assign({}, state, {loanType: action.payload})
        case UPDATE_PROPERTY_TYPE:
            return Object.assign({}, state, {propertyType: action.payload})
        case UPDATE_CITY:
            return Object.assign({}, state, {city: action.payload})

        default:
            return state
    }

} 

export function updateLoanType(loanType){
    return{
        type: UPDATE_LOAN_TYPE,
        payload: loanType
    }
}
export function updatePropertyType(property) {
    return {
        type: UPDATE_PROPERTY_TYPE,
        payload: property
    }
}

export function updateCity(city) {
    return {
        type: UPDATE_CITY,
        payload: city
    }
}

export default reducer; 
```

</details>

## Instructions 

In the `src/component/WizardTwo/WizardTwo.js`

* Import `connect` from `react-redux`.
* Import the update function you just made from your reducer: `updateCity` from `'./../../ducks/reducer'` (remember to destructure them). 
* Connect the WizardTwo component to `redux`, similarly to how we connected our `App.js` to `redux`.
    * Create a `mapStateToProps` function, passing it `state`.
    * Return an object that contains the piece of state you'll be updating/wanting access to.
    * In the export default we'll need to access the destructured functions from our reducer like so: `export default connect(mapStateToProps, { updateCity })(WizardTwo); `
* Now our component is connected to the `redux store`, let's access the function we need to change state on the input element.
    * Set the input element's `onChange` function equal to `{(e)=>this.props.updateCity(e.target.value)}`.
* Our WizardTwo Component should now be hooked up properly and be working with redux! 
<details>

<summary> Detailed Instructions </summary>

* Import `connect` from `react-redux`.

```js
import { connect } from 'react-redux'; 

```

* Import the update function you just made from your reducer: `updateCity` from `'./../../ducks/reducer'` (remember to destructure them). 

```js
import { updateCity } from './../../ducks/reducer'

```

* Connect the WizardTwo component to `redux`, similarly to how we connected our `App.js` to `redux`.
    * Create a `mapStateToProps` function, passing it `state`.
    * Return an object that contains the piece of state you'll be updating/wanting access to.
    * In the export default we'll need to access the destructured functions from our reducer like so: `export default connect(mapStateToProps, { updateCity })(WizardTwo); `
    
```js
function mapStateToProps( state ) {
  return { 
      city: state.city
    };
}
export default connect(mapStateToProps, { updateCity })(WizardTwo); 
```

* Now our component is connected to the `redux store`, let's access the function we need to change state on the input element.
    * Set the input element's `onChange` function equal to `{(e)=>this.props.updateCity(e.target.value)}`.
    * Because we've connected to `redux`, the updateLoanType function is now on props for this component.
* Our WizardTwo Component should now be hooked up properly and be working with redux! 
* You can see what's on state by writing `console.log(this.props)` inside of the `render()` function.
    
```js
<input placeholder="city name" type="text" onChange={(e)=>this.props.updateCity(e.target.value)}/>

```
</details>

### Solution

<details>

<summary> <code> src/components/WizardTwo/WizardTwo.js </code> </summary>


```js
import React,  { Component } from 'react';
import { Link } from 'react-router-dom';
import { updateCity } from './../../ducks/reducer'
import { connect } from 'react-redux'; 

class WizardTwo extends Component {
    render(){
        return(
            <div className="parent-div">
                <div className="vert-align">
            
                    <p>In what city will the property be located?</p><br />
                
                    <input placeholder="city name" type="text" onChange={(e)=>this.props.updateCity(e.target.value)}/>
               
                <Link to="/wThree"><button className="wTwo-btn"> Next </button></Link>
                </div>
            </div>
        )
    }
}

function mapStateToProps( state ) {
  return { 
      city: state.city
    };
}
export default connect(mapStateToProps, { updateCity })(WizardTwo); 
```

</details>



## Step 9
### Summary

Now that we have our second view hooked up. Let's move on to the third View.

## Instructions
In the `src/ducks/reducer.js`

* Create a new const: `const UPDATE_PROP  = 'UPDATE_PROP ';`
* Create an action for updating the prop:
    * Beneath the reducer function, export a function called `updatePropToBeUsedOn` and pass it `prop` as a parameter.
    * Return an object with a `type` equal to `UPDATE_PROP ` and a `payload` equal to `prop`.
* Create a reducer to update state for that action. Inside the reducer function:
    * Create a case for `UPDATE_PROP `
    * Return a new object that will become state, pass it empty curly braces, state, and the property with the value that you want to change.


<details>

<summary> Detailed Instructions </summary>

* Create a new const: `const UPDATE_PROP = 'UPDATE_PROP';`
    * We do this because react will throw an error if a variable is misspelled, but not if a string is misspelled.
```js
const UPDATE_PROP = 'UPDATE_PROP';

```

* Create an action for updating the prop:
    * All `actions` will return an object with a `type` and `payload`.
    * Beneath the reducer function, export a function called `updatePropToBeUsedOn` and pass it `prop` as a parameter.
    * Return an object with a `type` equal to `UPDATE_PROP` and a `payload` equal to `prop`.

```js
export function updatePropToBeUsedOn(prop){
    return {
        type: UPDATE_PROP,
        payload: prop
    }
}
```

* Create a reducer to update state for that action. Inside the reducer function:
    * Create a case for `UPDATE_PROP`
    * Return a new object that will become state, pass it empty curly braces, state, and the property with the value that you want to change.
    * Remember, Object.assign is used to copy values from an original source, the first parameter is curly brackets, showing that we want to make a `new` object, the second parameter is state, which is the object we want to copy all the values of, and the third parameter targets the specific property and it's value that we want to change on this new version of state.

```js
    case UPDATE_PROP:
        return Object.assign({}, state, {propToBeUsedOn: action.payload})
```

</details>


### Solution

<details>

<summary> <code> src/ducks/reducer.js </code> </summary>


```js
const initialState = {
   loanType: 'Home Purchase',
   propertyType: 'Single Family Home',
   city: '',
   propToBeUsedOn: '',
   found: false,
   realEstateAgent: "false",
   cost: 0,
   downPayment: 0,
   credit: '',
   history: '',
   addressOne: '',
   addressTwo: '',
   addressThree: '',
   firstName: 'aa',
   lastName: '',
   email: ''
}

const UPDATE_LOAN_TYPE = "UPDATE_LOAN_TYPE";
const UPDATE_PROPERTY_TYPE = 'UPDATE_PROPERTY_TYPE';
const UPDATE_CITY = 'UPDATE_CITY';
const UPDATE_PROP = 'UPDATE_PROP';

function reducer(state=initialState, action){ 

    switch(action.type){
        case UPDATE_LOAN_TYPE:
            return Object.assign({}, state, {loanType: action.payload})
        case UPDATE_PROPERTY_TYPE:
            return Object.assign({}, state, {propertyType: action.payload})
        case UPDATE_CITY:
            return Object.assign({}, state, {city: action.payload})
        case UPDATE_PROP:
            return Object.assign({}, state, {propToBeUsedOn: action.payload})

        default:
            return state
    }

} 

export function updateLoanType(loanType){
    return{
        type: UPDATE_LOAN_TYPE,
        payload: loanType
    }
}
export function updatePropertyType(property) {
    return {
        type: UPDATE_PROPERTY_TYPE,
        payload: property
    }
}

export function updateCity(city) {
    return {
        type: UPDATE_CITY,
        payload: city
    }
}

export function updatePropToBeUsedOn(prop){
    return {
        type: UPDATE_PROP,
        payload: prop
    }
}

export default reducer; 
```

</details>


## Instructions 

In the `src/component/WizardThree/WizardThree.js`

* Import `connect` from `react-redux`.
* Import the update function you just made from your reducer: `updatePropToBeUsedOn` from `'./../../ducks/reducer'` (remember to destructure them). 
* Connect the WizardThree component to `redux`, similarly to how we connected our `App.js` to `redux`.
    * Create a `mapStateToProps` function, passing it `state`.
    * Return an object that contains the piece of state you'll be updating/wanting access to.
    * In the export default we'll need to access the destructured functions from our reducer like so: `export default connect(mapStateToProps, { updatePropToBeUsedOn })(WizardThree); `
* Now our component is connected to the `redux store`, let's access the function we need to change state on the input element.
    * Set all of the button element's `onChange` functions equal to `{(e) =>this.props.updatePropToBeUsedOn(e.target.value)}`.
* Our WizardThree Component should now be hooked up properly and be working with redux! 
* You can see what's on state by writing `console.log(this.props)` inside of the `render()` function.
<details>

<summary> Detailed Instructions </summary>

* Import `connect` from `react-redux`.

```js
import { connect } from 'react-redux'; 

```

* Import the update function you just made from your reducer: `updatePropToBeUsedOn` from `'./../../ducks/reducer'` (remember to destructure them). 

```js
import { updatePropToBeUsedOn } from './../../ducks/reducer'

```

* Connect the WizardThree component to `redux`, similarly to how we connected our `App.js` to `redux`.
    * Create a `mapStateToProps` function, passing it `state`.
    * Return an object that contains the piece of state you'll be updating/wanting access to.
    * In the export default we'll need to access the destructured functions from our reducer like so: `export default connect(mapStateToProps, { updatePropToBeUsedOn })(WizardThree); `
    
```js
function mapStateToProps( state ) {
  return { 
      propToBeUsedOn: state.propToBeUsedOn
    };
}
export default connect(mapStateToProps, { updatePropToBeUsedOn })(WizardThree); 
```

* Now our component is connected to the `redux store`, let's access the function we need to change state on the input element.
    * Set all of the button element's `onChange` functions equal to `{(e) =>this.props.updatePropToBeUsedOn(e.target.value)}`.
    * Because we've connected to `redux`, the updateLoanType function is now on props for this component.
* Our WizardThree Component should now be hooked up properly and be working with redux! 
    
```js
<Link to="/wFour"><button value="Primary Home" onClick={(e) =>this.props.updatePropToBeUsedOn(e.target.value)}>Primary Home</button></Link>
<Link to="/wFour"><button value="Rental Property" onClick={(e) =>this.props.updatePropToBeUsedOn(e.target.value)}>Rental Property</button></Link>
<Link to="/wFour"><button value="Secondary Home" onClick={(e) =>this.props.updatePropToBeUsedOn(e.target.value)}>Secondary Home</button></Link>
```
</details>

### Solution

<details>

<summary> <code> src/components/WizardThree/WizardThree.js </code> </summary>


```js
import React,  { Component } from 'react';
import { Link } from 'react-router-dom';
import { updatePropToBeUsedOn } from './../../ducks/reducer'
import { connect } from 'react-redux'; 

class WizardThree extends Component {
    
    render(){
        return(
            <div className="parent-div">
                <div className="vert-align">
                <p>What property are you looking to use the loan on?</p> <br />
                    <div className="row">
                        <Link to="/wFour"><button value="Primary Home" onClick={(e) =>this.props.updatePropToBeUsedOn(e.target.value)}>Primary Home</button></Link>
                        <Link to="/wFour"><button value="Rental Property" onClick={(e) =>this.props.updatePropToBeUsedOn(e.target.value)}>Rental Property</button></Link>
                        <Link to="/wFour"><button value="Secondary Home" onClick={(e) =>this.props.updatePropToBeUsedOn(e.target.value)}>Secondary Home</button></Link>
                    </div>
                </div>        
            </div>
        )
    }
}

function mapStateToProps( state ) {
  return { 
      propToBeUsedOn: state.propToBeUsedOn
    };
}
export default connect(mapStateToProps, { updatePropToBeUsedOn })(WizardThree); 
```

</details>

## Step 10

### Summary

Now that we have our third view hooked up. Let's move on to the fourth View.

## Instructions 

In the `src/ducks/reducer.js`

* Create a new const: `const UPDATE_FOUND = 'UPDATE_FOUND';`
* Create an action for updating found:
    * Beneath the reducer function, export a function called `updateFound` and pass it `found` as a parameter.
    * Return an object with a `type` equal to `UPDATE_FOUND` and a `payload` equal to `found`.
* Create a reducer to update state for that action. Inside the reducer function:
    * Create a case for `UPDATE_FOUND`
    * Return a new object that will become state, pass it empty curly braces, state, and the property with the value that you want to change.


<details>

<summary> Detailed Instructions </summary>

* Create a new const: `const UPDATE_FOUND = 'UPDATE_FOUND';`
    * We do this because react will throw an error if a variable is misspelled, but not if a string is misspelled.
```js
const UPDATE_FOUND = 'UPDATE_FOUND';

```

* Create an action for updating the found:
    * All `actions` will return an object with a `type` and `payload`.
    * Beneath the reducer function, export a function called `updateFound` and pass it `found` as a parameter.
    * Return an object with a `type` equal to `UPDATE_FOUND` and a `payload` equal to `found`.

```js
export function updateFound(found){
    return {
        type: UPDATE_FOUND,
        payload: found
    }
}
```

* Create a reducer to update state for that action. Inside the reducer function:
    * Create a case for `UPDATE_FOUND`
    * Return a new object that will become state, pass it empty curly braces, state, and the property with the value that you want to change.
    * Remember, Object.assign is used to copy values from an original source, the first parameter is curly brackets, showing that we want to make a `new` object, the second parameter is state, which is the object we want to copy all the values of, and the third parameter targets the specific property and it's value that we want to change on this new version of state.

```js
    case UPDATE_FOUND:
        return Object.assign({}, state, {found: action.payload})
```
</details>

### Solution

<details>

<summary> <code> src/ducks/reducer.js </code> </summary>

```js
const initialState = {
   loanType: 'Home Purchase',
   propertyType: 'Single Family Home',
   city: '',
   propToBeUsedOn: '',
   found: false,
   realEstateAgent: "false",
   cost: 0,
   downPayment: 0,
   credit: '',
   history: '',
   addressOne: '',
   addressTwo: '',
   addressThree: '',
   firstName: 'aa',
   lastName: '',
   email: ''
}

const UPDATE_LOAN_TYPE = "UPDATE_LOAN_TYPE";
const UPDATE_PROPERTY_TYPE = 'UPDATE_PROPERTY_TYPE';
const UPDATE_CITY = 'UPDATE_CITY';
const UPDATE_PROP = 'UPDATE_PROP';
const UPDATE_FOUND = 'UPDATE_FOUND';


function reducer(state=initialState, action){ 

    switch(action.type){
        case UPDATE_LOAN_TYPE:
            return Object.assign({}, state, {loanType: action.payload})
        case UPDATE_PROPERTY_TYPE:
            return Object.assign({}, state, {propertyType: action.payload})
        case UPDATE_CITY:
            return Object.assign({}, state, {city: action.payload})
        case UPDATE_PROP:
            return Object.assign({}, state, {propToBeUsedOn: action.payload})
        case UPDATE_FOUND:
            return Object.assign({}, state, {found: action.payload})

        default:
            return state
    }

} 

export function updateLoanType(loanType){
    return{
        type: UPDATE_LOAN_TYPE,
        payload: loanType
    }
}
export function updatePropertyType(property) {
    return {
        type: UPDATE_PROPERTY_TYPE,
        payload: property
    }
}

export function updateCity(city) {
    return {
        type: UPDATE_CITY,
        payload: city
    }
}

export function updatePropToBeUsedOn(prop){
    return {
        type: UPDATE_PROP,
        payload: prop
    }
}

export function updateFound(found){
    return {
        type: UPDATE_FOUND,
        payload: found
    }
}

export default reducer; 
```

</details>

## Instructions 

In the `src/component/WizardFour/WizardFour.js`

* Import `connect` from `react-redux`.
* Import the update function you just made from your reducer: `updateFound` from `'./../../ducks/reducer'` (remember to destructure them). 
* Connect the WizardFour component to `redux`, similarly to how we connected our `App.js` to `redux`.
    * Create a `mapStateToProps` function, passing it `state`.
    * Return an object that contains the piece of state you'll be updating/wanting access to.
    * In the export default we'll need to access the destructured functions from our reducer like so: `export default connect(mapStateToProps, { updateFound })(WizardFour); `
* Now our component is connected to the `redux store`, let's access the function we need to change state on the input element.
    * Set the input element's `onChange` function equal to `{(e)=>this.props.updateFound(e.target.value)}`.
* Our WizardFour Component should now be hooked up properly and be working with redux! 
* You can see what's on state by writing `console.log(this.props)` inside of the `render()` function.
<details>

<summary> Detailed Instructions </summary>

* Import `connect` from `react-redux`.

```js
import { connect } from 'react-redux'; 

```

* Import the update function you just made from your reducer: `updateFound` from `'./../../ducks/reducer'` (remember to destructure them). 

```js
import { updateFound } from './../../ducks/reducer'

```

* Connect the WizardFour component to `redux`, similarly to how we connected our `App.js` to `redux`.
    * Create a `mapStateToProps` function, passing it `state`.
    * Return an object that contains the piece of state you'll be updating/wanting access to.
    * In the export default we'll need to access the destructured functions from our reducer like so: `export default connect(mapStateToProps, { updateFound })(WizardFour); `
    
```js
function mapStateToProps( state ) {
  return { 
      found: state.found
    };
}
export default connect(mapStateToProps, { updateFound })(WizardFour); 
```

* Now our component is connected to the `redux store`, let's access the function we need to change state on the input element.
    * Set the input element's `onChange` function equal to `{(e)=>this.props.updateFound(e.target.value)}`.
    * Because we've connected to `redux`, the updateLoanType function is now on props for this component.
* Our WizardFour Component should now be hooked up properly and be working with redux! 
    
```js
<input placeholder="found name" type="text" onChange={(e)=>this.props.updateFound(e.target.value)}/>

```
</details>

### Solution

<details>

<summary> <code> src/components/WizardFour/WizardFour.js </code> </summary>


```js
import React,  { Component } from 'react';
import { Link } from 'react-router-dom';
import { updateFound } from './../../ducks/reducer'
import { connect } from 'react-redux'; 


class WizardFour extends Component {
    render(){
        return(
            <div className="parent-div">
                <div className="vert-align">
               <p>Have you already found your new home?</p> <br />
                <div className="row">
                    <Link to="/wFive"><button onClick={ (e)=>this.props.updateFound("True") }>Yes</button></Link>
                    <Link to="/wFive"><button onClick={ (e)=>this.props.updateFound("False") }>No </button></Link> 
                </div>           
            </div>
        </div>
        )
    }
}

function mapStateToProps( state ) {
  return { 
      found: state.found
    };
}
export default connect(mapStateToProps, { updateFound })(WizardFour); 
```

</details>

## Step 11

### Summary

Now that we have our fourth view hooked up, I think we're getting the hang of the flow between the reducer and our component. So in this step we're going to finish out our reducer.js.

## Instructions 

In the `src/ducks/reducer.js`

* Create the following new const's: 

```js
const UPDATE_REALESTATE_AGENT = "UPDATE_REALESTATE_AGENT";
const UPDATE_COST = "UPDATE_COST";
const UPDATE_DOWNPAYMENT = "UPDATE_DOWNPAYMENT";
const UPDATE_CREDIT = "UPDATE_CREDIT";
const UPDATE_HIST = "UPDATE_HIST";
const UPDATE_ADD_ONE = "UPDATE_ADD_ONE";
const UPDATE_ADD_TWO = "UPDATE_ADD_TWO";
const UPDATE_ADD_THREE = "UPDATE_ADD_THREE";
const UPDATE_FIRST_NAME = "UPDATE_FIRST_NAME";
const UPDATE_LAST_NAME = "UPDATE_LAST_NAME";
const UPDATE_EMAIL = "UPDATE_EMAIL"; 
```

* Create actions for updating the following properties:
    * realEstateAgent
    * cost
    * downPayment
    * credit
    * history
    * addressOne
    * addressTwo
    * addressThree
    * firstName
    * lastName
    * email
* Remember to export each function, and that each function returns an object with two properties: `type` and `payload`.
* Each function will have a parameter being passed into it that contains the value for `payload`, it's what we'll be updating the property on state to. 
* `type` will equal the case you want your action to match

<details>

<summary> Detailed Instructions </summary>

* Create actions for updating the following properties:
    * realEstateAgentalse
    * cost
    * downPayment
    * credit
    * history
    * addressOne
    * addressTwo
    * addressThree
    * firstNamea'
    * lastName
    * email
* Remember to export each function, and that each function returns an object with two properties: `type` and `payload`.
* Each function will have a parameter being passed into it that contains the value for `payload`, it's what we'll be updating the property on state to. 
* `type` will equal the case you want your action to match

```js
export function updateLoanType(loanType){
    return{
        type: UPDATE_LOAN_TYPE,
        payload: loanType
    }
}
export function updatePropertyType(property) {
    return {
        type: UPDATE_PROPERTY_TYPE,
        payload: property
    }
}

export function updateCity(city) {
    return {
        type: UPDATE_CITY,
        payload: city
    }
}

export function updatePropToBeUsedOn(prop){
    return {
        type: UPDATE_PROP,
        payload: prop
    }
}
export function updateFound(found){
    return {
        type: UPDATE_FOUND,
        payload: found
    }
}

export function updateRealEstateAgent(bool){
    return {
        type: UPDATE_REALESTATE_AGENT,
        payload: bool
    }    
}

export function updateCost(num){
    return {
        type: UPDATE_COST,
        payload: num
    }    
}

export function updateDownPayment(num){
    return {
        type: UPDATE_DOWNPAYMENT,
        payload: num
    }    
}

export function updateCredit(num){
    return {
        type: UPDATE_CREDIT,
        payload: num
    }    
}

export function updateHist(hist){
    return {
        type: UPDATE_HIST,
        payload: hist
    }    
}

export function updateAddOne(add){
    return {
        type: UPDATE_ADD_ONE,
        payload: add
    }    
}
export function updateAddTwo(add){
    return {
        type: UPDATE_ADD_TWO,
        payload: add
    }    
}
export function updateAddThree(add){
    return {
        type: UPDATE_ADD_THREE,
        payload: add
    }    
}

export function updateFirstName(first){
    return {
        type: UPDATE_FIRST_NAME,
        payload: first
    }    
}

export function updateLastName(last){
    return {
        type: UPDATE_LAST_NAME,
        payload: last
    }    
}

export function updateEmail(email){
    return {
        type: UPDATE_EMAIL,
        payload: email
    }    
}
```

</details>

* Moving on to our reducers, you'll need to make cases that will activate depending on which action.type is passed through the switch function.  
* Create a case for each action type
    * In each case, return a new object that will become state, pass it empty curly braces, state, and the property with the value that you want to change.
    * Remember, Object.assign is used to copy values from an original source, the first parameter is curly brackets, showing that we want to make a `new` object, the second parameter is state, which is the object we want to copy all the values of, and the third parameter targets the specific property and it's value that we want to change on this new version of state.

<details>

<summary> Detailed Instructions </summary>

* Moving on to our reducers, you'll need to make cases that will activate depending on which action.type is passed through the switch function.  
* Create a case for each action type
    * In each case, return a new object that will become state, pass it empty curly braces, state, and the property with the value that you want to change.
    * Remember, Object.assign is used to copy values from an original source, the first parameter is curly brackets, showing that we want to make a `new` object, the second parameter is state, which is the object we want to copy all the values of, and the third parameter targets the specific property and it's value that we want to change on this new version of state.

```js
function reducer(state=initialState, action){ 

    switch(action.type){
        case UPDATE_LOAN_TYPE:
            return Object.assign({}, state, {loanType: action.payload})
        case UPDATE_PROPERTY_TYPE:
            return Object.assign({}, state, {propertyType: action.payload})
        case UPDATE_CITY:
            return Object.assign({}, state, {city: action.payload})
        case UPDATE_PROP:
            return Object.assign({}, state, {propToBeUsedOn: action.payload})
        case UPDATE_FOUND:
            return Object.assign({}, state, {found: action.payload})
        case UPDATE_REALESTATE_AGENT:
            return Object.assign({}, state, {realEstateAgent: action.payload})
        case UPDATE_COST:
            return Object.assign({}, state, {cost: action.payload})
        case UPDATE_DOWNPAYMENT:
            return Object.assign({}, state, {downPayment: action.payload})
        case UPDATE_CREDIT:
            return Object.assign({}, state, {credit: action.payload})
        case UPDATE_HIST:
            return Object.assign({}, state, {history: action.payload})
        case UPDATE_ADD_ONE:
            return Object.assign({}, state, {addressOne: action.payload})
        case UPDATE_ADD_TWO:
            return Object.assign({}, state, {addressTwo: action.payload})
        case UPDATE_ADD_THREE:
            return Object.assign({}, state, {addressThree: action.payload})
        case UPDATE_FIRST_NAME:
            return Object.assign({}, state, {firstName: action.payload})
        case UPDATE_LAST_NAME:
            return Object.assign({}, state, {lastName: action.payload})
        case UPDATE_EMAIL:
            return Object.assign({}, state, {email: action.payload})

        default:
            return state
    }
} 
```

</details>


### Solution

<details>

<summary> <code> src/ducks/reducer.js </code> </summary>

```js
const initialState = {
   loanType: 'Home Purchase',
   propertyType: 'Single Family Home',
   city: '',
   propToBeUsedOn: '',
   found: false,
   realEstateAgent: "false",
   cost: 0,
   downPayment: 0,
   credit: '',
   history: '',
   addressOne: '',
   addressTwo: '',
   addressThree: '',
   firstName: 'aa',
   lastName: '',
   email: ''
}

const UPDATE_LOAN_TYPE = "UPDATE_LOAN_TYPE";
const UPDATE_PROPERTY_TYPE = 'UPDATE_PROPERTY_TYPE';
const UPDATE_CITY = 'UPDATE_CITY';
const UPDATE_PROP = 'UPDATE_PROP';
const UPDATE_FOUND = 'UPDATE_FOUND';
const UPDATE_REALESTATE_AGENT = "UPDATE_REALESTATE_AGENT";
const UPDATE_COST = "UPDATE_COST";
const UPDATE_DOWNPAYMENT = "UPDATE_DOWNPAYMENT";
const UPDATE_CREDIT = "UPDATE_CREDIT";
const UPDATE_HIST = "UPDATE_HIST";
const UPDATE_ADD_ONE = "UPDATE_ADD_ONE";
const UPDATE_ADD_TWO = "UPDATE_ADD_TWO";
const UPDATE_ADD_THREE = "UPDATE_ADD_THREE";
const UPDATE_FIRST_NAME = "UPDATE_FIRST_NAME";
const UPDATE_LAST_NAME = "UPDATE_LAST_NAME";
const UPDATE_EMAIL = "UPDATE_EMAIL"; 

function reducer(state=initialState, action){ 

    switch(action.type){
        case UPDATE_LOAN_TYPE:
            return Object.assign({}, state, {loanType: action.payload})
        case UPDATE_PROPERTY_TYPE:
            return Object.assign({}, state, {propertyType: action.payload})
        case UPDATE_CITY:
            return Object.assign({}, state, {city: action.payload})
        case UPDATE_PROP:
            return Object.assign({}, state, {propToBeUsedOn: action.payload})
        case UPDATE_FOUND:
            return Object.assign({}, state, {found: action.payload})
        case UPDATE_REALESTATE_AGENT:
            return Object.assign({}, state, {realEstateAgent: action.payload})
        case UPDATE_COST:
            return Object.assign({}, state, {cost: action.payload})
        case UPDATE_DOWNPAYMENT:
            return Object.assign({}, state, {downPayment: action.payload})
        case UPDATE_CREDIT:
            return Object.assign({}, state, {credit: action.payload})
        case UPDATE_HIST:
            return Object.assign({}, state, {history: action.payload})
        case UPDATE_ADD_ONE:
            return Object.assign({}, state, {addressOne: action.payload})
        case UPDATE_ADD_TWO:
            return Object.assign({}, state, {addressTwo: action.payload})
        case UPDATE_ADD_THREE:
            return Object.assign({}, state, {addressThree: action.payload})
        case UPDATE_FIRST_NAME:
            return Object.assign({}, state, {firstName: action.payload})
        case UPDATE_LAST_NAME:
            return Object.assign({}, state, {lastName: action.payload})
        case UPDATE_EMAIL:
            return Object.assign({}, state, {email: action.payload})



        default:
            return state
    }

} 

export function updateLoanType(loanType){
    return{
        type: UPDATE_LOAN_TYPE,
        payload: loanType
    }
}
export function updatePropertyType(property) {
    return {
        type: UPDATE_PROPERTY_TYPE,
        payload: property
    }
}

export function updateCity(city) {
    return {
        type: UPDATE_CITY,
        payload: city
    }
}

export function updatePropToBeUsedOn(prop){
    return {
        type: UPDATE_PROP,
        payload: prop
    }
}
export function updateFound(found){
    return {
        type: UPDATE_FOUND,
        payload: found
    }
}

export function updateRealEstateAgent(bool){
    return {
        type: UPDATE_REALESTATE_AGENT,
        payload: bool
    }    
}

export function updateCost(num){
    return {
        type: UPDATE_COST,
        payload: num
    }    
}

export function updateDownPayment(num){
    return {
        type: UPDATE_DOWNPAYMENT,
        payload: num
    }    
}

export function updateCredit(num){
    return {
        type: UPDATE_CREDIT,
        payload: num
    }    
}

export function updateHist(hist){
    return {
        type: UPDATE_HIST,
        payload: hist
    }    
}

export function updateAddOne(add){
    return {
        type: UPDATE_ADD_ONE,
        payload: add
    }    
}
export function updateAddTwo(add){
    return {
        type: UPDATE_ADD_TWO,
        payload: add
    }    
}
export function updateAddThree(add){
    return {
        type: UPDATE_ADD_THREE,
        payload: add
    }    
}

export function updateFirstName(first){
    return {
        type: UPDATE_FIRST_NAME,
        payload: first
    }    
}

export function updateLastName(last){
    return {
        type: UPDATE_LAST_NAME,
        payload: last
    }    
}

export function updateEmail(email){
    return {
        type: UPDATE_EMAIL,
        payload: email
    }    
}
export default reducer; 
```

</details>

## Step 12

### Summary

Setting up the fifth view with our reducer.

## Instructions 

In the `src/component/WizardFive/WizardFive.js`

* Import `connect` from `react-redux`.
* Import the update function you just made from your reducer: `updateRealEstateAgent ` from `'./../../ducks/reducer'` (remember to destructure them). 
* Connect the WizardFive component to `redux`, similarly to how we connected our `App.js` to `redux`.
    * Create a `mapStateToProps` function, passing it `state`.
    * Return an object that contains the piece of state you'll be updating/wanting access to.
    * In the export default we'll need to access the destructured functions from our reducer like so: `export default connect(mapStateToProps, { updateRealEstateAgent  })(WizardFive); `
* Now our component is connected to the `redux store`, let's access the function we need to change state on the input element.
    * Set the first button element's `onChange` function equal to `{(e)=>this.props.updateRealEstateAgent("True")}`.
    * Set the second button element's `onChange` function equal to `{(e)=>this.props.updateRealEstateAgent("False")}`.
* Our WizardFive Component should now be hooked up properly and be working with redux! 
* You can see what's on state by writing `console.log(this.props)` inside of the `render()` function.
<details>

<summary> Detailed Instructions </summary>

* Import `connect` from `react-redux`.

```js
import { connect } from 'react-redux'; 

```

* Import the update function you just made from your reducer: `updateRealEstateAgent ` from `'./../../ducks/reducer'` (remember to destructure them). 

```js
import { updateRealEstateAgent  } from './../../ducks/reducer'

```

* Connect the WizardFive component to `redux`, similarly to how we connected our `App.js` to `redux`.
    * Create a `mapStateToProps` function, passing it `state`.
    * Return an object that contains the piece of state you'll be updating/wanting access to.
    * In the export default we'll need to access the destructured functions from our reducer like so: `export default connect(mapStateToProps, { updateRealEstateAgent  })(WizardFive); `
    
```js
function mapStateToProps( state ) {
  return { 
      realEstateAgent: state.realEstateAgent
    };
}
export default connect(mapStateToProps, { updateRealEstateAgent  })(WizardFive); 
```

* Now our component is connected to the `redux store`, let's access the function we need to change state on the input element.
    * Set the first button element's `onChange` function equal to `{(e)=>this.props.updateRealEstateAgent("True")}`.
    * Set the second button element's `onChange` function equal to `{(e)=>this.props.updateRealEstateAgent("False")}`.    
    * Because we've connected to `redux`, the updateLoanType function is now on props for this component.
* Our WizardFive Component should now be hooked up properly and be working with redux! 
    
```js
<Link to="/wSix"><button onClick={ (e)=>this.props.updateRealEstateAgent("True") }>Yes</button></Link>
<Link to="/wSix"><button onClick={ (e)=>this.props.updateRealEstateAgent("False") }>No </button></Link>
```
</details>

### Solution

<details>

<summary> <code> src/components/WizardFive/WizardFive.js </code> </summary>


```js
import React,  { Component } from 'react';
import { Link } from 'react-router-dom';
import { updateRealEstateAgent } from './../../ducks/reducer'
import { connect } from 'react-redux';

class WizardFive extends Component {

    render(){
        return(
            <div className="parent-div">
                <div className="vert-align">

                    <p>Are you currently working with a real estate agent?</p> <br />
                    <div className="row">
                        <Link to="/wSix"><button onClick={ (e)=>this.props.updateRealEstateAgent("True") }>Yes</button></Link>
                        <Link to="/wSix"><button onClick={ (e)=>this.props.updateRealEstateAgent("False") }>No </button></Link>
                    </div>
                </div>
            </div>
        )
    }
}

function mapStateToProps( state ) {
  return { 
      found: state.found
    };
}
export default connect(mapStateToProps, { updateRealEstateAgent })(WizardFive); 
```

</details>


## Step 13

### Summary

Setting up the sixth view with our reducer.

## Instructions 

In the `src/component/WizardSix/WizardSix.js`

* Import `connect` from `react-redux`.
* Import the update functions you just made from your reducer: `updateCost, updateDownPayment ` from `'./../../ducks/reducer'` (remember to destructure them). 
* Connect the WizardSix component to `redux`, similarly to how we connected our `App.js` to `redux`.
    * Create a `mapStateToProps` function, passing it `state`.
    * Return an object that contains the piece of state you'll be updating/wanting access to.
    * In the export default we'll need to access the destructured functions from our reducer like so: `export default connect(mapStateToProps, { updateCost, updateDownPayment })(WizardSix);`
* Now our component is connected to the `redux store`, let's access the function we need to change state on the input element.
    * Set the first input element's `onChange` function equal to `{ (e)=> this.props.updateCost(e.target.value) }`.
    * Set the second input element's `onChange` function equal to `{ (e)=> this.props.updateDownPayment(e.target.value) }`.
* Our WizardSix Component should now be hooked up properly and be working with redux! 
* You can see what's on state by writing `console.log(this.props)` inside of the `render()` function.
<details>

<summary> Detailed Instructions </summary>

* Import `connect` from `react-redux`.

```js
import { connect } from 'react-redux'; 

```

* Import the update functions you just made from your reducer: `updateCost, updateDownPayment ` from `'./../../ducks/reducer'` (remember to destructure them)

```js
import { updateCost, updateDownPayment } from './../../ducks/reducer';

```

* Connect the WizardSix component to `redux`, similarly to how we connected our `App.js` to `redux`.
    * Create a `mapStateToProps` function, passing it `state`.
    * Return an object that contains the piece of state you'll be updating/wanting access to.
    * In the export default we'll need to access the destructured functions from our reducer like so: `export default connect(mapStateToProps, { updateCost, updateDownPayment })(WizardSix);`
    
```js
function mapStateToProps( state ) {
  return { 
      cost: state.cost,
      downPayment: state.downPayment
    };
}
export default connect(mapStateToProps, { updateCost, updateDownPayment })(WizardSix); 
```

* Now our component is connected to the `redux store`, let's access the function we need to change state on the input element.
    * Set the first input element's `onChange` function equal to `{ (e)=> this.props.updateCost(e.target.value) }`.
    * Set the second input element's `onChange` function equal to `{ (e)=> this.props.updateDownPayment(e.target.value) }`.  
    * Because we've connected to `redux`, the updateLoanType function is now on props for this component.
* Our WizardSix Component should now be hooked up properly and be working with redux! 
    
```js
<input type="text" placeholder="amount" onChange={ (e)=> this.props.updateCost(e.target.value) }/> <br />
<input type="text" placeholder="amount" onChange={ (e)=> this.props.updateDownPayment(e.target.value) }/>
```
</details>

### Solution

<details>

<summary> <code> src/components/WizardSix/WizardSix.js </code> </summary>


```js
import React,  { Component } from 'react';
import { Link } from 'react-router-dom';
import { updateCost, updateDownPayment } from './../../ducks/reducer'
import { connect } from 'react-redux'; 

class WizardSix extends Component {
    render(){
        return(
            <div className="parent-div">
                <div className="vert-align">
                    <p>What is the estimated purchase price?</p> <br />
                        
                        
                        <input type="text" placeholder="amount" onChange={ (e)=> this.props.updateCost(e.target.value) }/> <br />


                    <p>How much are you putting down as a down payment?</p> <br />
                        
                        
                        <input type="text" placeholder="amount" onChange={ (e)=> this.props.updateDownPayment(e.target.value) }/>
                        
                    
                    <Link to="/wSeven"><button className="wSix-btn"> Next </button></Link>
                </div>
            </div>
        )
    }
}


function mapStateToProps( state ) {
  return { 
      cost: state.cost,
      downPayment: state.downPayment
    };
}
export default connect(mapStateToProps, { updateCost, updateDownPayment })(WizardSix); 
```

</details>

## Step 14

### Summary

Hooking up the rest of our views to the reducer.

## Instructions 

Continue to go through the rest of the views and hook them up to the reducer like we've done in previous steps. When you're done, you should be able to see all of the data you've passed in the eleventh view.

### Solution For Seventh View

<details>

<summary> <code> src/components/WizardSeven/WizardSeven.js </code> </summary>


```js
import React,  { Component } from 'react';
import { Link } from 'react-router-dom';
import { updateCredit } from './../../ducks/reducer'
import { connect } from 'react-redux'; 

class WizardSeven extends Component {

    render(){
        return(
            <div className="parent-div">
                <div className="vert-align">
                    <p>Estimate your credit score</p> <br />
                    <div className="row">
                        <Link to="/wEight"><button onClick={ (e)=>this.props.updateCredit('Excellent') }>Excellent</button></Link>
                        <Link to="/wEight"><button onClick={ (e)=>this.props.updateCredit('Good') }>Good</button></Link>
                        <Link to="/wEight"><button onClick={ (e)=>this.props.updateCredit('Fair') }>Fair</button></Link>
                        <Link to="/wEight"><button onClick={ (e)=>this.props.updateCredit('Poor') }>Poor</button></Link>
                    </div>
                </div>
            </div>
        )
    }
}

function mapStateToProps( state ) {
  return { 
      credit: state.credit,
    };
}
export default connect(mapStateToProps, { updateCredit })(WizardSeven); 
```

</details>


### Solution For Eigth View
   
<details>

<summary> <code> src/components/WizardEight/WizardEight.js </code> </summary>


```js
import React,  { Component } from 'react';
import { Link } from 'react-router-dom';
import { updateHist } from './../../ducks/reducer'
import { connect } from 'react-redux'; 

class WizardEight extends Component {

    render(){
        return(
            <div className="parent-div">
                <div className="vert-align">
                    <p>Have you had a bankruptcy or foreclosure in the past seven years?</p> <br />
                <div className="row">
                    <Link to="/wNine"><button value="Has never been in bankruptcy" onClick={ (e)=> this.props.updateHist(e.target.value) }>No</button></Link>
                    <Link to="/wNine"><button value="Has had bankruptcy before" onClick={ (e)=> this.props.updateHist(e.target.value) }>Bankruptcy</button></Link>
                    <Link to="/wNine"><button value="Has had a foreclosure before" onClick={ (e)=> this.props.updateHist(e.target.value) }>Foreclosure</button></Link>
                    <Link to="/wNine"><button value="Has had both a foreclosure and a bankruptcy" onClick={ (e)=> this.props.updateHist(e.target.value) }>Both</button></Link>
                </div>    
                </div>
            </div>
        )
    }
}

function mapStateToProps( state ) {
  return { 
      history: state.history,
    };
}
export default connect(mapStateToProps, { updateHist })(WizardEight); 
```

</details>

### Solution For Ninth View

<details>

<summary> <code> src/components/WizardNine/WizardNine.js </code> </summary>


```js
import React,  { Component } from 'react';
import { Link } from 'react-router-dom';
import { updateAddOne, updateAddTwo, updateAddThree } from './../../ducks/reducer'
import { connect } from 'react-redux'; 

class WizardNine extends Component {

    render(){
        return(
            <div className="parent-div">
                <div className="vert-align">
                    <p>What is your address?</p> <br />
                    <input type="text" placeholder="Line One" onChange={ (e)=>this.props.updateAddOne(e.target.value) }/>
                    <input type="text" placeholder="Line Two" onChange={ (e)=>this.props.updateAddTwo(e.target.value) }/>
                    <input type="text" placeholder="Line Three" onChange={ (e)=>this.props.updateAddThree(e.target.value) }/>
               
                
                    <Link to="/wTen"><button className="margin-btn"> Next </button></Link>
                </div>
            </div>
        )
    }
}
function mapStateToProps( state ) {
  return { 
     addressOne: state.addressOne,
     addressTwo: state.addressTwo,
     addressThree: state.addressThree
  };
}
export default connect(mapStateToProps, { updateAddOne, updateAddTwo, updateAddThree })(WizardNine); 
```

</details>

### Solution For Tenth View

<details>

<summary> <code> src/components/WizardTen/WizardTen.js </code> </summary>


```js
import React,  { Component } from 'react';
import { Link } from 'react-router-dom';
import { updateFirstName, updateLastName, updateEmail } from './../../ducks/reducer'
import { connect } from 'react-redux'; 

class WizardTen extends Component {
    render(){
        return(
            <div className="parent-div">
                <div className="vert-align">
                    <p>What is your name and Email?</p> <br />
                    <input type="text" placeholder="First Name" onChange={ (e)=>this.props.updateFirstName(e.target.value) }/>
                    <input type="text" placeholder="Last Name" onChange= { (e)=>this.props.updateLastName(e.target.value) }/>
                    <input type="text" placeholder="email" onChange={ (e)=>this.props.updateEmail(e.target.value) }/>
                    
                    <Link to="/wEleven"><button className="margin-btn"> Next </button></Link>
                </div>
            </div>
        )
    }
}
function mapStateToProps( state ) {
  return { 
     firstName: state.firstName,
     lastName: state.lastName,
     email: state.email
  };
}
export default connect(mapStateToProps, { updateFirstName, updateLastName, updateEmail })(WizardTen); 
```

</details>

## Contributions

If you see a problem or a typo, please fork, make the necessary changes, and create a pull request so we can review your changes and merge them into the master repo and branch.

## Copyright

© DevMountain LLC, 2017. Unauthorized use and/or duplication of this material without express and written permission from DevMountain, LLC is strictly prohibited. Excerpts and links may be used, provided that full and clear credit is given to DevMountain with appropriate and specific direction to the original content. 
<br />
<img src="https://devmounta.in/img/logowhiteblue.png" width="250" align="right">
