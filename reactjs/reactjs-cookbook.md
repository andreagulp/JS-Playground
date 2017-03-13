# Reactjs Cookbook

## Command line - delete file

```sh
cd src && rm App.css App.js App.test.js favicon.ico index.css logo.svg
```

## datatime formatting with momentjs

```javascript
{moment(issue.created).fromNow()}
```

## Conditional render jsx with inline if

```javascript
              {issue.e2eCycleTime !== 'Invalid date' ? <TableRowColumn>{issue.e2eCycleTime}</TableRowColumn> :  <TableRowColumn>started {moment(issue.created).fromNow()}</TableRowColumn>}

```

## Call APi with axios and make statistical calculation and store them in a new array

  ```javascript
    import React, { Component } from 'react';
    import './App.css';

    import CardStat001 from '.././components/CardStat001';
    import TableGit from '.././components/TableGit';

    import axios from 'axios'
    import moment from 'moment';

    class App extends Component {
      constructor (props) {
        super (props);
        this.state = {
          gitHubBaseUrl: 'https://github.ibm.com/api/v3/repos/EMEA-Accelerate/Transition-Team/issues?access_token=d6ce7c2a96a7146f1da65e4392c37d19db1a7a7f&per_page=100&state=all&page=0',
          gitIssues: [],
          cycleTimeData: [],
        }
      }

    // API call
      getGit = () => {
        axios.get(this.state.gitHubBaseUrl)
                      .then(response => {
                        response = response.data;
                        this.setState({gitIssues: response});
                        let tempCycleTimeData = this.state.gitIssues.map((x) => {
                          return {
                            issueNum: x.number,
                            issueTitle: x.title,
                            issueStatus: x.state,
                            created: x.created_at,
                            closed: x.closed_at,
                            e2eCycleTime: moment.utc(moment(x.closed_at).diff(moment(x.created_at))).format("HH:mm:ss"),
                            startWeekNum: moment(x.created_at).week(),
                            closedWeekNum: moment(x.closed_at).week()
                          }
                        })
                        this.setState({
                          cycleTimeData: tempCycleTimeData
                        })
                      })
      }

      componentWillMount = () => {
        this.getGit();
        console.log(Date.now());
      }


      render() {

    // Calculations
        let issuesCount = this.state.gitIssues.length
        let issuesCountClosed = this.state.gitIssues.filter(x => x.state === 'closed').length
        let issuesCountOpen = this.state.gitIssues.filter(x => x.state === 'open').length
        let percOpen = Math.floor((issuesCountOpen / issuesCount) * 100) + '%'

        return (
          <div>

            <CardStat001
              issuesCount={issuesCount}
              issuesCountOpen={issuesCountClosed}
              issuesCountClosed={issuesCountOpen}
              percOpen={percOpen}
            />
            <TableGit
              cycleTimeData={this.state.cycleTimeData}
            />
          </div>
          );
        }
      }

    export default App;
  
  ```

## Use Filter to count category

  ```javascript
  let issuesCountClosed = this.state.gitIssues.filter(x => x.state === 'closed').length
  ```




## Use a defulat value for input field

  1. Parent Component
  
  ```javascript
      this.state = {
        currentTime: moment.duration(25, 'minutes'),
        baseTime: moment.duration(25, 'minutes')
      }
    }

    {...}

      <TimerConfig
        baseTime={this.state.baseTime}
      />  

      {...}

  ```

  2. Child Component
  
  ```javascript
  <input id="hours" className="form-control" type="number" defaultValue={props.baseTime.get('hours')}/>
  <input id="minutes" className="form-control" type="number" defaultValue={props.baseTime.get('minutes')}/>
  <input id="seconds" className="form-control" type="number" defaultValue={props.baseTime.get('seconds')}/>
  ```



## Clear Input field with react-bootstrap <formControl>

  1.  If you need to access the value of an uncontrolled <FormControl>, attach a ref to it as you would with an uncontrolled input, then call ReactDOM.findDOMNode(ref) to get the DOM node. You can then interact with that node as you would with any other uncontrolled input.
  
  ```javascript
    addRisk = (e) => {
    e.preventDefault();
    this.props.addRisk();
    ReactDOM.findDOMNode(this.refs.title).value = '';
    ReactDOM.findDOMNode(this.refs.description).value = '';
  }
  
  {...}
  
      <FormControl
        type="text"
        placeholder="title"
        ref="title"
        onChange={this.props.handleChangeTitle}
      />

    {...}
      <FormControl
        componentClass="textarea"
        placeholder="description"
        ref="description"
        onChange={this.props.handleChangeDescription}
      />
  ```
  
  
## Import Material UI for react (http://www.material-ui.com)

  1. Install Material UI + required module
  
  ```
  npm i -s material-ui
  npm i -s react-tap-event-plugin
  ```
  
  2. Import injectTapEventPlugin in Index.js
  
  ```javascript
  import injectTapEventPlugin from 'react-tap-event-plugin';
  injectTapEventPlugin();
  ```
  
  3. Import MuiThemeProvider in app container (e.g App.js)
  
  ```javascript
  import MuiThemeProvider from 'material-ui/styles/MuiThemeProvider';
  ```
  
  ```javascript
      <MuiThemeProvider>
        <ChildComponent />
      </MuiThemeProvider>
  ```
  

## Call Api with axios and set to an array in this.state

  ```sh
  npm i -s axios
  ```

  ```javascript
    {...}
    import axios from 'axios'
    {...}
    componentDidMount () {
    axios.get(this.state.gitHubBaseUrl)
                  .then(response => {
                    this.setState({gitIssuesList: response.data});
                  })
  }
  ```
  
  
  
## Component, PureComponent, Stateless functional Component

  1. Component
  
  ```javascript
    import React, { Component } from 'react';

    class Header extends Component {
      constructor (props) {
        super (props);
        this.state = {}
      }
      render () {
        return (
            <div className="App-header">
              <h1>Risk Log</h1>
            </div>
        )
      }
    };
    export default Header  
  ```

  
  2. PureComponents
  
  ```javascript
    import React, { PureComponent } from 'react';

    class Header extends PureComponent {
      constructor (props) {
        super (props);
        this.state = {}
      }
      render () {
        return (
            <div className="App-header">
              <h1>Risk Log</h1>
            </div>
        )
      }
    };
    export default Header
  ```
  
  3. Stateless Functional Component
  
  ```javascript
    import React from 'react';

    const CardStat001 = (props) => {

        return (
            <div>
                <p>All Issues: {props.issuesCount}</p>
                <p>Open Issues: {props.issuesCountOpen}</p>
                <p>Closed Issues: {props.issuesCountClosed}</p>
            </div>
        )
    };
    export default CardStat001
  ```
  
  
  
## Basic Search in a list of items

```javascript
import React, { Component } from 'react';

class App extends Component {
  constructor (props) {
    super (props);
    this.state = {
      contacts: [
        {id: 1, name: 'andrea', phone: '09856390'},
        {id: 2, name: 'mario', phone: '1234567890'},
        {id:3, name: 'giuseppe', phone: '0987654321'},
      ],
      search: ''
    }
  }

  addContact = (e) => {
    e.preventDefault();
    let name = this.refs.name.value;
    let phone = this.refs.phone.value;
    let id = Math.floor((Math.random() * 100) + 1);
    this.setState({
      contacts: this.state.contacts.concat({id, phone, name})
    })
    this.refs.name.value = '';
    this.refs.phone.value = '';
  }

  updateSearch = (e) => {
    this.setState({
      search: e.target.value
    })
  }

  render () {
    let filteredContacts = this.state.contacts.filter((contact) => contact.name.indexOf(this.state.search) !== -1)

    return (
        <div>
          <h1>App Component Working</h1>
          <input type="text" value={this.state.search} onChange={this.updateSearch} />
          <form onSubmit={this.addContact}>
            <input type="text" ref="name" autoFocus/>
            <input type="text" ref="phone" />
            <button type="submit">Add New Contact</button>
          </form>
          <ul>
            {filteredContacts.map((contact, i) => <li key={contact.id}>{contact.name} | {contact.phone}</li>)}
          </ul>
        </div>
    )
  }
};
export default App
```

  
## Clean Input Field

  1. in App.js create a function that receives whatever value is in the input field. Pass it down until the component should use it
  
  ```javascript
    handleChange = (e) => {
    let tempItem = e.target.value;
    this.setState({newItem: {title: tempItem, isClosed: false}})
    }
  ```
  
  2. In the method that adds the item to the array, add a code that cleans the binded property of the input field
  
  ```javascript
    addItem = (e) => {
      e.preventDefault();
      this.state.items.push(this.state.newItem);
      this.setState({
        items: this.state.items,
        newItem: {title: ''}
      })
      this.updateLocalTodos(this.state.items);
    }
  ```
  
  

## Dynamic link in img

```javascript
import logo from './../assets/logo.svg';

[...]
        <img src={logo}/>
```


## Delete item from array with onClick event

  1. in App.js, the state has a property `items: [{title: 'a title for item'}]` and `deleteItem` method
  2. `deleteItem()` takes an `index`(see later for explanation). `index` is used by `splice()` function to eliminate 1 element starting at the position of `index`, then `setState` making items = to items
  
  ```javascript
  class App extends Component {
  constructor (props) {
    super (props);
    this.state = {
      items: this.getLocalTodos() || itemsDB,
      newItem: {title: ''}
    }
  }
  
    deleteItem = (index) => {
    this.state.items.splice(index, 1);
    this.setState({items: this.state.items});
    this.updateLocalTodos(this.state.items)
  }
  
  ```
  
  3. `this.state.items` and `deleteItem(index)` are passed down App.js -> ItemList.js -> Item.js
    
    3.1. From App.js -> ItemList.js

    ```javascript
      render() {
      return (
          <div className="container">
            <ItemList
              items={this.state.items}
              deleteItem={(index) => this.deleteItem(index)}
            />
          </div>
      );
    }
    ```
  
    3.2. From ItemList.js -> Item.js
    
    ```javascript
        <ul className="event-list">
          {this.props.items.map((item, i) => 
            <Item 
              key={i} 
              item={item} 
              index={i} 
              deleteItem={(index) => this.props.deleteItem(i)} 
            />)}
        </ul>
    ```
  
  4. In Item.js, use a function to store `index` props
  ```javascript
    deleteItem = () => {
      this.props.deleteItem(this.props.index);
  }
  ```
  
  5. Now we can use the new created function with a click event
  
  ```javascript
      <li className="facebook" >
        <a href="#facebook">
          <span 
            className="todo-delete fa fa-trash" 
            onClick={() => this.deleteItem()}>
          </span>
        </a>
      </li>
  ```
  
  
  

## Tutorials
- [codecademy.com] (https://www.codecademy.com/)
- Pluralsight
- [#ReactForNewbies: Building a Todo App with Create-React-App [Part 1]] (https://edemkumodzi.com/reactfornewbies-building-a-todo-app-with-create-react-app-part-1-5aae4bd637ee#.yq4qnarna)
- [#ReactForNewbies: Building a Todo App with Create-React-App [Part 2]] (https://edemkumodzi.com/reactfornewbies-building-a-todo-app-with-create-react-app-part-2-f846e2d8b820#.ll0rd3fid)
- [#ReactForNewbies: Building a Todo App with Create-React-App [Part 3]] (https://edemkumodzi.com/reactfornewbies-building-a-todo-app-with-create-react-app-part-3-e735065521b#.l8elp7t8l)
- [#ReactForNewbies: Building a Todo App with Create-React-App [Part 4]] (https://edemkumodzi.com/reactfornewbies-building-a-todo-app-with-create-react-app-part-4-c87cd06876e3#.46zc4q2qf)

## Implement Local Storage
```javascript
import React, { Component } from 'react';

import ItemList from './ItemList';
import itemsDB from '.././assets/itemsDB';
import Header from './Header';
import AddItemForm from './AddItemForm';

class App extends Component {
  constructor (props) {
    super (props);
    this.state = {
      items: this.getLocalTodos() || itemsDB,
      newItem: {title: ''}
    }
  }

  handleChange = (e) => {
    let tempItem = e.target.value;
    this.setState({newItem: {title: tempItem}})
  }

  addItem = () => {
    this.state.items.push(this.state.newItem);
    this.setState({
      items: this.state.items
    })
  }
  
  componentDidUpdate = () => {
    this.updateLocalTodos(this.state.items);
  }

  render() {
    return (
        <div className="container">
          <AddItemForm handleChange={this.handleChange} addItem={this.addItem}/>
          <Header />
          <ItemList items={this.state.items}/>
        </div>
    );
  }

  updateLocalTodos = (todos) => {
    if (!window.localStorage) {
        console.log("Your browser dose NOT support localStorage!");
        return;
    }
    localStorage["IssueRisk-Log-Storage"] = JSON.stringify(todos);
  }

  getLocalTodos = () => {
      if (!window.localStorage) {
          console.log("Your browser dose NOT support localStorage!");
          return "";
      }
      if (localStorage["IssueRisk-Log-Storage"]) {
          console.log(JSON.parse(localStorage["IssueRisk-Log-Storage"]));
          return JSON.parse(localStorage["IssueRisk-Log-Storage"]);
      } else {
          return "";
      }
    }

}

export default App;
```


## Import bootstrap 3, theme, fonteawesome Method 1


  1. Install Bootstrap 3 and Fontawesome
  
  ```sh
  npm i -s bootstrap
  npm i -s font-awesome
  ```
  
  2. Import in Index.js
  
  ```javascript
  import 'bootstrap/dist/css/bootstrap.css';
  import 'bootstrap/dist/css/bootstrap-theme.css';
  import 'font-awesome/css/font-awesome.css';
  ```
  
## Import bootstrap and jquery Method 2 (using require)

  1. install bootstrap and jquery
  
  ```sh
  npm i bootstrap jquery -s
  ```
  
  2. import in index.js and require bootstrap and jquery
  
  ```javascript
    global.jQuery = require('jquery');
    require('bootstrap');
    import 'bootstrap/dist/css/bootstrap.css'
  ```


## Use create-react-app command
command to create app
```sh
create-react-app my-project
```

## Import external library

  1. cmd project folder and install library
  
    ```sh
    npm i moment --save
    ```
    
  2. import the library (could be index.js or app.js to make available globally or a specific component)
  
    ```javascript
    import Moment from 'moment';
    ```
    
## render an array of items with .map

  - App.js
  
  ```javascript
    import React, { Component } from 'react';
    import './App.css';

    import TaskList from './components/TaskList';

    class App extends Component {
      constructor (props) {
        super (props);
        this.state = {
          tasks: [
            {id: 1, name: 'Do this and That'},
            {id: 2, name: 'Buy some Milk'}
          ],

          userInputTaskName: ''
        }
      }


      render() {
        return (
          <div className="App">
            <TaskList tasks={this.state.tasks} />
          </div>
        );
      }
    }

    export default App;
```

  - TaskList.js
  
  ```javascript
    import React, { Component } from 'react';

    import Task from './Task';

    class TaskList extends Component {
      constructor (props) {
        super (props);
        this.state = {}
      }
      render () {
        return (
            <div>
              <ul>
                {this.props.tasks.map((task, i) => <Task task={task} key={i}/>)}
              </ul>
            </div>
        )
      }
    };
    export default TaskList
  ```

  - Task.js
  
  ```javascript
    import React, { Component } from 'react';

    class Task extends Component {
      constructor (props) {
        super (props);
        this.state = {}
      }
      render () {
        return (
            <div>
              <span>{this.props.task.id} - </span>
              <label>{this.props.task.name}  </label>
            </div>
        )
      }
    };
    export default Task
  ```


## Bind 1 Input Field with onChange

  1. App.js
  use onChange event on the input field to track if the field changes. if yes the handleInputChange() will take the target.value of the   element input, store it on a temporary variable and then use it to setState.
  
  ```javascript
    import React, { Component } from 'react';
    import './App.css';

    import TaskForm from './components/TaskForm';
    import TaskList from './components/TaskList';
    import AddTaskButton from './components/AddTaskButton';

    class App extends Component {
      constructor (props) {
        super (props);
        this.state = {
          tasks: [
            {id: 1, name: 'Do this and That'},
            {id: 2, name: 'Buy some Milk'}
          ],
          userInputName: ''
        }
      }

      addTask = (e) => {
        let newTask = {id: this.state.tasks.length+1, name: this.state.userInputName};
        let newTasks = this.state.tasks.concat(newTask);
        this.setState({tasks: newTasks})
      }

      handleinputChange = (e) => {
        let newUserInputName = e.target.value;
        this.setState({userInputName: newUserInputName})
      }

      render() {
        return (
          <div className="App">
            <Header />
            <TaskForm handleinputChange={this.handleinputChange} userInputName={this.userInputName}/>
            <AddTaskButton addTask={this.addTask}/>
            <TaskList tasks={this.state.tasks} />
          </div>
        );
      }
    }

    export default App;
  ```
  
  2. TaskForm.js
  
  ```javascript
    import React, { Component } from 'react';

    class TaskForm extends Component {
      constructor (props) {
        super (props);
        this.state = {}
      }
      render () {
        return (
            <div>
              <h1>TaskForm Component Working</h1>
              <input value={this.props.userInputName} onChange={this.props.handleinputChange} />
            </div>
        )
      }
    };
    export default TaskForm
  ```
  
## Bind and add multiple input field with onChange + clear field
  
  1. App.js container holds the array and the methods
  
  ```javascript
    import React, { Component } from 'react';

    import './App.css';
    import Risks from '.././assets/RisksDB';

    import Header from './../components/Header';
    import RiskList from '.././components/RiskList';
    import AddRiskForm from './../components/AddRiskForm';

    class App extends Component {
      constructor (props) {
        super (props);
        this.state = {
          risks: this.getLocalStorage() || Risks,
          tempTitle: '',
          tempDescription: '',
        }
      }

      handleChangeTitle = (e) => {
        let newTitle = e.target.value;
        this.setState({tempTitle:newTitle})
      }

      handleChangeDescription = (e) => {
        let newDescription = e.target.value;
        this.setState({tempDescription:newDescription})
      }

      addRisk = () => {
        let id = Date.now();
        let title = this.state.tempTitle;
        let description = this.state.tempDescription;
        let isOpen = true;
        this.setState({
          risks: this.state.risks.concat({id, title, description, isOpen}),
          tempTitle: '',
          tempDescription: ''
        })
        this.updateLocalStorage(this.state.risks)
      }

      updateLocalStorage = (risks) => {
        if (!window.localStorage) {
          return console.log('Your Browser doesn\'t support Local Storage. Sorry we cannot save your risk. Try to update your browser to latest version');
        } else {
          return localStorage["Risk-Log-002"] = JSON.stringify(risks)
        }
      }

      getLocalStorage = () => {
        return JSON.parse(localStorage["Risk-Log-002"]);
      }

      render() {
        return (
          <div className="App">
            <Header />
            <AddRiskForm
              addRisk={this.addRisk}
              handleChangeTitle={this.handleChangeTitle}
              handleChangeDescription={this.handleChangeDescription}
            />
            <RiskList risks={this.state.risks}/>
          </div>
        );
      }
    }

    export default App;
  ```
  
  2. AddRiskForm.js pure component calls addRisk inside local addRisk function (the local function could be called in any way). This is usefull as the local function beside calling the function in App.js, can be used to do other local operation, like use refs to clean the field and event.prevent default. It could also be used to pass parameters to the App.js function (in this case when used in render() needs to be ```onClick={() => {this.props.addRisk(param)}}```
  
  ```javascript
    import React, { Component } from 'react';

    import './addRiskForm.css'

    class AddRiskForm extends Component {
      constructor (props) {
        super (props);
        this.state = {}
      }

      addRisk = (e) => {
        e.preventDefault();
        this.props.addRisk();
        this.refs.title.value = '';
        this.refs.description.value = '';
      }

      render () {
        return (
          <form className="form-horizontal">
            <div className="form-group">
              <label className="col-sm-2 control-label">Risk Title</label>
              <div className="col-sm-10">
                <input
                  type="text"
                  className="form-control"
                  placeholder="Risk Title"
                  autoFocus
                  ref="title"
                  onChange={this.props.handleChangeTitle}
                />
              </div>
            </div>
            <div className="form-group">
              <label className="col-sm-2 control-label">Risk Description</label>
              <div className="col-sm-10">
                <textarea
                  className="form-control"
                  rows="3"
                  placeholder="Risk Description"
                  ref="description"
                  onChange={this.props.handleChangeDescription}
                />
              </div>
            </div>
            <div className="form-group">
              <div className="col-sm-offset-2 col-sm-10">
                <button type="submit" className="btn btn-default" onClick={this.addRisk}>Sign in</button>
              </div>
            </div>
          </form>
            )
      }
    };
    export default AddRiskForm  
  ```
 



## Add element to Array

  1. See example above (Bind Input Field). Notice function addTask(). The function builds a newTask based on the user task name and the id (see in the code to understand how it is derived as unique value). At the end it creates a new Array of tasks which = oldArray.concat(newObject)
  
  ```javascript
    addTask = (e) => {
      let newTask = {id: this.state.tasks.length+1, name: this.state.userInputName};
      let newTasks = this.state.tasks.concat(newTask);
      this.setState({tasks: newTasks})
    }
  ```

  2. Button to add the task
  
  ```javascript
    import React, { Component } from 'react';

    class AddTaskButton extends Component {
      constructor (props) {
        super (props);
        this.state = {}
      }
      render () {
        return (
            <div>
              <button onClick={this.props.addTask}>+</button>
            </div>
        )
      }
    };
    export default AddTaskButton
  ```

## Add multiple inputs field to an array of object using refs

```javascript
import React, { Component } from 'react';

class App extends Component {
  constructor (props) {
    super (props);
    this.state = {
      contacts: [
        {id: 1, name: 'andrea', phone: '09856390'},
        {id: 2, name: 'mario', phone: '1234567890'},
        {id:3, name: 'giuseppe', phone: '0987654321'},
      ]
    }
  }

  addContact = (e) => {
    e.preventDefault();
    let name = this.refs.name.value;
    let phone = this.refs.phone.value;
    let id = Math.floor((Math.random() * 100) + 1);
    this.setState({
      contacts: this.state.contacts.concat({id, phone, name})
    })
    console.log(this.state.contacts);
  }

  render () {
    return (
        <div>
          <h1>App Component Working</h1>
          <form onSubmit={this.addContact}>
            <input type="text" ref="name" />
            <input type="text" ref="phone" />
            <button type="submit">Add New Contact</button>
          </form>
          <ul>
            {this.state.contacts.map((contact, i) => <li key={i}>{contact.name} | {contact.phone}</li>)}
          </ul>
        </div>
    )
  }
};
export default App
```
