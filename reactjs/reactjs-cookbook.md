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


## Nex Topic
