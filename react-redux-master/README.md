# Getting Started with Create React App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `yarn start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in your browser.

The page will reload when you make changes.\
You may also see any lint errors in the console.

### `yarn test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `yarn build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `yarn eject`

**Note: this is a one-way operation. Once you `eject`, you can't go back!**

If you aren't satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you're on your own.

You don't have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn't feel obligated to use this feature. However we understand that this tool wouldn't be useful if you couldn't customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: [https://facebook.github.io/create-react-app/docs/code-splitting](https://facebook.github.io/create-react-app/docs/code-splitting)

### Analyzing the Bundle Size

This section has moved here: [https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size](https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size)

### Making a Progressive Web App

This section has moved here: [https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app](https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app)

### Advanced Configuration

This section has moved here: [https://facebook.github.io/create-react-app/docs/advanced-configuration](https://facebook.github.io/create-react-app/docs/advanced-configuration)

### Deployment

This section has moved here: [https://facebook.github.io/create-react-app/docs/deployment](https://facebook.github.io/create-react-app/docs/deployment)





<h2>Creating the increment counter application using Redux in React</h2>
1. Install the redux and react-redux libraries for your application using the below command. Once installed, verify if the required packages are installed in the package.json file.

`npm install redux react-redux --no-audit`

2. In this application that we are building to learn the use of redux with react, we will have one MainPanel component which contains two internal Components, MyButton and DivPanel.

3. MyButton is a button component which maintains a counter onClick. The value of this counter will be displayed in DivPanel. The content of DivPanel will be automatically refreshed everytime the counter value changes.

4. Under the src folder, create a folder named action to define the actions for our application. The only action you are going to perform is incrementing of the counter. In the action folder, create index.js and paste the following code given below.


<code>const increment = (val) => {
    return {
        type : 'INCREMENT',
        inc : val
    }
}
export default increment; </code>

val is the value you want to increase the counter by everytime the button is clicked. Now that you have your action which defines what is to be done, you will create the reducers which will define how it is done.

5. Under the src folder, create a folder named reducers. In the reducers folder create index.js and paste the code given below.
<code>
import {combineReducers} from 'redux'
const counter = (state=0,action)=>{
    if(action.type === 'INCREMENT') {
        //This will increase the value of counter by the value passed to the increment method
        return state+action.inc;
    }
    //Returns the current value of the counter
    return state;
}
const myReducers = combineReducers({counter});
export default myReducers;
</code>

6. Now you have your action and reducers. What is left to be created is the store. Before you create the store you will create the components. Create a folder for the components named components inside the src folder. Create MyButton.js file inside the component folder and paste the code given below.
<code>
import React from 'react'
import { useDispatch} from 'react-redux';
import increment from '../action'
const MyButton = ()=>{
    let dispatch = useDispatch();
    return (
        <button onClick={()=>dispatch(increment(1))}>Increase counter</button>
    );
}
export default MyButton;
</code>

useDispatch dispatches the event to the store and finds out what action is to be taken and uses the appropriate reducer to do the same.

7. You will now create the DivPanel.js file inside the components folder which will contain DivPanel where you will display the counter value.
<code>
import React from 'react'
import { useSelector } from 'react-redux';
const DivPanel = () =>{
    let counterVal = useSelector(state => state.counter)
    return (
        <div>
            The present value of counter is {counterVal}
        </div>
    );
}
export default DivPanel;
</code>

useSelector is used to select the state from the store whose value you want to access.

8. Now we will create the MainPanel.js with the two components in the file MainPanel.js.
<code>
import React from 'react'
import MyButton from './MyButton'
import DivPanel from './DivPanel';
const MainPanel = ()=>{
    return (
        <div>
            This is main panel <MyButton></MyButton>
            <DivPanel></DivPanel>
        </div>
    );
}
export default MainPanel;
</code>

9. You have all the panels created. Now let’s render the MainPanel through App.js. App.js contains the code give below.
<code>
import React from 'react';
import MainPanel from './components/MainPanel';
function App() {
  return (
      <div>
        <MainPanel/>
      </div>
    );
}
export default App;
</code>

10. Now for the final set up of the react application. You need to create and set up the store, where you can manage all the states (in this application, the counter) you want. This is done in index.js.
<code>
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import {Provider} from 'react-redux'
import myReducers from './reducers'
import {legacy_createStore as createStore} from 'redux';;
//Create the store
const myStore = createStore(myReducers);
//This will console log the current state everytime the state changes
myStore.subscribe(()=>console.log(myStore.getState()));
//Enveloping the App inside the Provider, ensures that the states in the store are available
//throughout the application
ReactDOM.render(<Provider store={myStore}><App/></Provider>, document.getElementById('root'));
</code>

11. In the terminal, ensure you are in the react-redux directory and run the following command to start the server and run the application.

`npm start`

You will see this output indicating that the server is running.

12. To verify that the server is running, click on the Skills Network button on the left to open the Skills Network Toolbox. Then click Other. Choose Launch Application and enter the port number 3000 on which the server is running and click the launch icon.


The increment counter application using Redux will appear on the browser as seen in the image below.Check the application by incrementing the counter.


13. To stop the server, go to the terminal in the lab environment and press Ctrl+c to stop the server.

<h2>Congratulations! You have completed the lab for creating increment counter Application using Redux in React.</h2>

### `yarn build` fails to minify

This section has moved here: [https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify](https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify)
