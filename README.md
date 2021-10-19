# react-development
# React Context: How to Use the useContext Hook 
Why Context?
In React, the components are the building blocks of the product. These components are defined in the tree hierarchy where one component is the parent of the child component.

 

The data flow in react is down the hierarchy i.e. the data flow from parent component to child component and further down. If the component is deep down in the hierarchy from the root component then the data is passed through all the middle components first and then it will be accessible to the component that is deep down the hierarchy.

 

To prevent this type of design some reactjs developers use redux as a library that works on the global store that stores the data or state globally and is accessible to all the components directly without passing data or state down the hierarchy.

 

Context is another very curious and awesome way introduced by react to store the data or state globally. 

 

For implementing react context in your project:
 

1. Create a file to create a context object and export that context object from that file.
 

theme-context.js


import React from ‘react’

export const ThemeContext = React.createContext( ‘dark’ );
 

Here we have created and exported a context object. Context object accepts a default parameter or value ( default props ) that can be passed to it. In case if no props are passed by the parent component then it takes default props defined in the context object.

 

A react context object always comes with a provider function. In the next step, we will see what a provider is.

 

2. Now in a parent component whose values are to be passed to child component.  
 

We will wrap any child component that is going down the hierarchy in a react context provider.

Note: If any child component wants to access the state or value from ancestor than the parent or grandparent of that child component should be wrapped inside a react context provider.

 

Component A  —>  Component B  —->   Component C ——> Component D

  (ancestor)     (child 1)     (child 2)         (child 3)

 

Here Component A is the root component, component B is the child of component A, component C is the child of component B, component D is the child of component C.

 

If child 3 wants to access the data from the ancestor or component A than any of component B, C or D should be wrapped in provider.

 

The Child who wants to access the data, its any parent, or itself should be wrapped inside the context provider of the ancestor that pass the data. 

 

app.js

import {ThemeContext, themes} from './theme-context';

import ThemedButton from './themed-button';


// An intermediate component that uses the ThemedButton

function Toolbar(props) {

  return (

    <ThemedButton onClick={props.changeTheme}>

       Change Theme

    </ThemedButton>

  );

}

class App extends React.Component {

  constructor(props) {

    super(props);

    this.state = {

      theme: themes.light,

    };


    this.toggleTheme = () => {

      this.setState(state => ({

        theme:

          state.theme === themes.dark

            ? themes.light

            : themes.dark,

      }));

    };

  }


  render() {


   // The ThemedButton button inside the ThemeProvider


   // uses the theme from the state while the one outside uses


   // the default dark theme


   return (

      <Page>


       <ThemeContext.Provider value={this.state.theme}>


          <Toolbar changeTheme={this.toggleTheme} />


       </ThemeContext.Provider>


       <Section>


          <ThemedButton />


       </Section>

      </Page>

    );

  }

}


ReactDOM.render(<App />, document.root);
 

Here class App is the ancestor. The Toolbar is a child functional component and ThemedButton is also a child component.

 

The Toolbar is wrapped inside the context provider. The context provider passes the values that can be accessible to Toolbar and its any child component. ThemedButton is not wrapped inside any of the context providers, therefore, it will take the default value of the context.

 

The value passed by the provider to the child component can be accessible by it or any child of that child component.

 

Note: Any child component wrapped inside the provider will take the values passed and if the child component is not wrapped inside the provider it will use the default value of the react context provider.

 

So we are done with parent component, now let’s move on to the child component definition.

 

3. In child components, we can access the values in multiple ways. 
 

Let’s discuss the first way to access the value in child component (contextType).

 

Themed-button.js


Before Context


import {ThemeContext} from './theme-context';


class ThemedButton extends React.Component {

  render() {

    let props = this.props;


   let theme = this.props; // this.props used


   return (

      <button

        {...props}

        style={{backgroundColor: theme.background}}

      />

    );

  }

}


ThemedButton.contextType = ThemeContext;

export default ThemedButton;


After Context


import {ThemeContext} from './theme-context';

import React from ‘react’


class ThemedButton extends React.Component {

  render() {

    let props = this.props;


   let theme = this.context; //Replaced by this.context


   return (

      <button

        {...props}

        style={{backgroundColor: theme.background}}

      />

    );

  }

}


ThemedButton.contextType = ThemeContext;


export default ThemedButton;
 

Here ThemedButton is the child component. If you want to access the value then make use contextType before exporting the component. After that everywhere you have used this.props can be replaced by this.context.  

 

Another way of accessing values is by using react hooks.

 

import {ThemeContext} from './theme-context';

import React, {useContext} from ‘react’


class ThemedButton extends React.Component {

  render() {

  
   let theme = useContext(ThemeContext);


   return (

      <button

        {...props}

        style={{backgroundColor: theme.background}}

      />

    );

  }

}

export default ThemedButton;
 

Here we made use of hooks useContext. After this, we can directly access the properties as shown above.This was all about Context.To know more,<a href="https://www.cronj.com/react/react-development-agency" rel="nofollow">react development agency</a>

 
