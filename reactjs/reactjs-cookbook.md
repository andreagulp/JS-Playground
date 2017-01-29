# Reactjs Cookbook

## Tutorials
- [codecademy.com] (https://www.codecademy.com/)
- Pluralsight
- [#ReactForNewbies: Building a Todo App with Create-React-App [Part 1]] (https://edemkumodzi.com/reactfornewbies-building-a-todo-app-with-create-react-app-part-1-5aae4bd637ee#.yq4qnarna)
- [#ReactForNewbies: Building a Todo App with Create-React-App [Part 2]] (https://edemkumodzi.com/reactfornewbies-building-a-todo-app-with-create-react-app-part-2-f846e2d8b820#.ll0rd3fid)
- [#ReactForNewbies: Building a Todo App with Create-React-App [Part 3]] (https://edemkumodzi.com/reactfornewbies-building-a-todo-app-with-create-react-app-part-3-e735065521b#.l8elp7t8l)
- [#ReactForNewbies: Building a Todo App with Create-React-App [Part 4]] (https://edemkumodzi.com/reactfornewbies-building-a-todo-app-with-create-react-app-part-4-c87cd06876e3#.46zc4q2qf)


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

  See example above (Bind Input Field. Notice function
  
  ```javascript
    addTask = (e) => {
      let newTask = {id: this.state.tasks.length+1, name: this.state.userInputName};
      let newTasks = this.state.tasks.concat(newTask);
      this.setState({tasks: newTasks})
    }
  ```