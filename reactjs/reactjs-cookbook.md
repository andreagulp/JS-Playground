# Reactjs Cookbook

## Questions that need answers
  - How to clear an input field? - DONE
  - How to bind multiple field and add them to an array list? - DONE
  
  
  
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

    const Header = () => {
      return (
          <div className="App-header">
            <h1>Risk Log</h1>
          </div>
      )
    }

    export default Header
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


## Import bootstrap 3, theme, fonteawesome

```sh
import 'bootstrap/dist/css/bootstrap.css';
import 'bootstrap/dist/css/bootstrap-theme.css';
import 'font-awesome/css/font-awesome.css';
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


## Bind Input Field

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
