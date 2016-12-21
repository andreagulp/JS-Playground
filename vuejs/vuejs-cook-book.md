#Vue js  Cook Book

## Import axios in vue cli webpack applicaton
  
  1. Save axios ``` npm install axios --save ```
  2. import in the .vue file where you use ``` import axios from 'axios' ```


## LinkedIn API. Component to get job position list from user LinkedIn profile

```vue
<template lang="html">

  <div class="container-fluid">

    <div id="timeline">

      <div class="row timeline-movement timeline-movement-top">
        <div class="timeline-badge timeline-future-movement">
          <a href="#">
            <span class="glyphicon glyphicon-plus"></span>
          </a>
        </div>
        <div class="timeline-badge timeline-filter-movement">
          <a href="#">
            <span class="glyphicon glyphicon-time"></span>
          </a>
        </div>

      </div>

      <div class="row timeline-movement" v-for="item in jobPositions">

        <div class="timeline-badge">
          <span class="timeline-balloon-date-day" v-if="item.endDate"> {{item.endDate.year}} </span>
          <span class="timeline-balloon-date-month" >{{item.startDate.year}} </span>
        </div>


        <div class="col-sm-6  timeline-item">
          <div class="row">
            <div class="col-sm-11">
              <div class="timeline-panel credits">
                <ul class="timeline-panel-ul">
                  <li><span class="importo">{{item.title}}</span></li>
                  <li><span class="causale">{{item.company.name}} @ {{item.location.name}}</span> </li>
                  <li v-if="item.isCurrent">
                    <p><small class="text-muted"><i class="glyphicon glyphicon-time"></i> Currently working here</small></p>
                  </li>
                </ul>
              </div>

            </div>
          </div>
        </div>

        <div class="col-sm-6  timeline-item">
            <div class="row">
                <div class="col-sm-offset-1 col-sm-11">
                    <div class="timeline-panel debits">
                        <ul class="timeline-panel-ul">
                            <li><span class="importo">Mussum ipsum cacilds</span></li>
                            <li><span class="causale">Lorem ipsum dolor sit amet, consectetur adipiscing elit. </span> </li>
                            <li><p><small class="text-muted"><i class="glyphicon glyphicon-time"></i> 11/09/2014</small></p> </li>
                        </ul>
                    </div>

                </div>
            </div>
        </div>
      </div>
      <!--due -->
    </div>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    data() {
      return {
        msg: 'Positions',
        jobPositions: '',
        linkedInBaseUrl: 'https://api.linkedin.com/v1/people/~:(positions)?oauth2_access_token=xxxxxxxxxxxx&format=json'
      }
    },

    created() {
      axios.get(this.linkedInBaseUrl)
        .then(response => {
          this.jobPositions = response.data.positions.values;
        })
    },

    methods: {

    }
  }
</script>

<style lang="css">

  #timeline {
  list-style: none;
  position: relative;
  }
  #timeline:before {
  top: 0;
  bottom: 0;
  position: absolute;
  content: " ";
  width: 2px;
  background-color: #4997cd;
  left: 50%;
  margin-left: -1.5px;
  }
  #timeline .clearFix {
  clear: both;
  height: 0;
  }
  #timeline .timeline-badge {
  color: #fff;
  width: 50px;
  height: 50px;
  font-size: 1.2em;
  text-align: center;
  position: absolute;
  top: 20px;
  left: 50%;
  margin-left: -25px;
  background-color: #4997cd;
  z-index: 100;
  border-top-right-radius: 50%;
  border-top-left-radius: 50%;
  border-bottom-right-radius: 50%;
  border-bottom-left-radius: 50%;
  }
  #timeline .timeline-badge span.timeline-balloon-date-day {
  font-size: .7em;
  }
  #timeline .timeline-badge span.timeline-balloon-date-month {
  font-size: .7em;
  position: relative;
  top: -10px;
  }
  #timeline .timeline-badge.timeline-filter-movement {
  background-color: #ffffff;
  font-size: 1.7em;
  height: 35px;
  margin-left: -18px;
  width: 35px;
  top: 40px;
  }
  #timeline .timeline-badge.timeline-filter-movement a span {
  color: #4997cd;
  font-size: 1.3em;
  top: -1px;
  }
  #timeline .timeline-badge.timeline-future-movement {
  background-color: #ffffff;
  height: 35px;
  width: 35px;
  font-size: 1.7em;
  top: -16px;
  margin-left: -18px;
  }
  #timeline .timeline-badge.timeline-future-movement a span {
  color: #4997cd;
  font-size: .9em;
  top: 2px;
  left: 1px;
  }
  #timeline .timeline-movement {
  border-bottom: dashed 1px #4997cd;
  position: relative;
  }
  #timeline .timeline-movement.timeline-movement-top {
  height: 60px;
  }
  #timeline .timeline-movement .timeline-item {
  padding: 20px 0;
  }
  #timeline .timeline-movement .timeline-item .timeline-panel {
  border: 1px solid #d4d4d4;
  border-radius: 3px;
  background-color: #FFFFFF;
  color: #666;
  padding: 10px;
  position: relative;
  -webkit-box-shadow: 0 1px 6px rgba(0, 0, 0, 0.175);
  box-shadow: 0 1px 6px rgba(0, 0, 0, 0.175);
  }
  #timeline .timeline-movement .timeline-item .timeline-panel .timeline-panel-ul {
  list-style: none;
  padding: 0;
  margin: 0;
  }
  #timeline .timeline-movement .timeline-item .timeline-panel.credits .timeline-panel-ul {
  text-align: right;
  }
  #timeline .timeline-movement .timeline-item .timeline-panel.credits .timeline-panel-ul li {
  color: #666;
  }
  #timeline .timeline-movement .timeline-item .timeline-panel.credits .timeline-panel-ul li span.importo {
  color: #468c1f;
  font-size: 1.3em;
  }
  #timeline .timeline-movement .timeline-item .timeline-panel.debits .timeline-panel-ul {
  text-align: left;
  }
  #timeline .timeline-movement .timeline-item .timeline-panel.debits .timeline-panel-ul span.importo {
  color: #e2001a;
  font-size: 1.3em;
  }
</style>

```

## Create a Github repo from Local repository

Adding an existing project to GitHub using the command line
From <https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/> 

 1. ``` git init ```
 2. ``` git add . ```
 3. ``` git commit -m "First commit" ```
 4. ``` git remote add origin remote repository URL ```
 5. ``` git remote -v ```
 6. ``` git push origin master ```
 

format of push command:
``` git push  <REMOTENAME> <BRANCHNAME> ```


## Add bootstrap.css to a vue-cli webpack installation

  1. ```npm install bootstrap@3 --save```
  2. then import in main.js
  
  ```javascript
  //main.js
  import Vue from 'vue'
  import App from './App'
  import '../node_modules/bootstrap/dist/css/bootstrap.css'

  /* eslint-disable no-new */
  new Vue({
    el: '#app',
    template: '<App/>',
    components: { App }
  })
```




## Atom snipets

  1. Tab stops are defined using a dollar followed by a number, i.e. $1, $2, $3 etc. 
  2. Single quotes within the body must be escaped with a preceding backslash (\').
  3. The snippet scope in snippets.cson must also have a period added to the start of that string. Popular web-language scopes include:

    - HTML: .text.html.basic
    - CSS: .source.css
    - SASS: .source.sass
    - JavaScript: .source.js
    - JSON: .source.json
    - PHP: .text.html.php
    - Java: .source.java
    - Ruby: .text.html.erb
    - Python: .source.python
    - plain text (including markdown): .text.plain
    
  4. multi-line snippets can be defined using three double-quotes """ at the start and end of the body code. 
      
      Multi-line Snippet Body

>      '.source.js':
>        'if, else if, else':
>          'prefix': 'ieie'
>          'body': """
>            if (${1:true}) {
>              $2
>            } else if (${3:false}) {
>              $4
>            } else {
>              $5
>            }
>          """


## Togle mark all checkboxes

```html
<button type="button" name="button" v-on:click="markAll">Clear List</button>
```

```javascript
markAll () {
  for (var i = 0; i < this.taskList.length; i++) {
    if (this.taskList[i].checked) {
      this.taskList[i].checked = false ;
    } else {
      this.taskList[i].checked = true;
    }
  }
}
```


## v-bind with a css class

```css
.list li.done label {
  color: #d9d9d9;
  text-decoration: line-through;
}
```

```html
<ul class="list">
<li v-for="task in taskList" v-bind:class="{done: task.checked}">
  <input type="checkbox" name="" value="" class="checkbox" v-model="task.checked">
  <label for="checkbox">{{task.text}}</label>
  <button type="button" name="button" class="delete" v-on:click="removeTask(task)">x</button>
</li>
</ul>
```



## Create Components

1. ```vue init webpack-simple <project-name>```
2. Create a new file named AddTaskBtn.vue in components folder

    ```html
    <template>
        <button type="button" class="btn-add" >+++</button>
    </template>

    <script>
    export default {
        data() {
            return {

            }
        }
    }
    </script>

    <style>
        .btn-add {color: #42b983;}

    </style>
    ```
    
3. Add the component to root component App.vue
 
 ``` html
 <template>
    <div id="app">

    <div class="row">
      <div class="col-md-8 col-md-offset-2">
         <h2>Awesome Todo</h2> 05 December, 2016

         <AddTaskBtn></AddTaskBtn>

      </div>
    </div>

  </div>
</template>

<script>

import AddTaskBtn from './components/AddTaskBtn.vue';

export default {
  name: 'app',
  components: { AddTaskBtn },
}

</script>

<style>
a { color: #42b983;}
</style>
```




## Intialize basic js project with npm
 1. Create Project folder
 2. Gitbash the folder
 3. `npm init`
 4. `npm install vue vue-resource bootstrap`
 5. `touch index.html app.js`

## Generate basic HTML boilerplate with emmet
```html
! [tab]
```

## Add vue.js CDN

```html
<script src="https://unpkg.com/vue/dist/vue.js"></script>
```

## Http get call from api end point

  **NOTE:** Currently i can have this working only with vuejs 1
  
  1. Import Vue resource. e.g with CDN
  ``` html
  <body>
  
      <script src="https://cdn.jsdelivr.net/vue.resource/1.0.3/vue-resource.min.js"></script>
  </body>
```
  2. in methods, build function to get result from api end point
  
  ``` javascript
  // vue.js
  
  var vm1 = new Vue({
    el: '#app1',
    data: {
        contacts: [],
        apiPosts: [],
    },

    ready: function() {
        this.getEntries();
    },

    methods: {
        getEntries() {
            var apiUrl = 'https://jsonplaceholder.typicode.com/posts';

            this.$http.get(apiUrl)
                .then((response) => this.apiPosts.push(response));
        }
    }
})
  ```
  
  ``` html
  <!-- vue.html -->
        <div>
            <ol>
                <li v-for="post in apiPosts">{{post.title}}</li>
            </ol>
        </div>
  ```

## Flatten array of array in promises
 > api call to [https://jsonplaceholder.typicode.com/posts] 
 > already retrives an array that is .put inside another array
 > e.g.
```json
   "apiPosts": [
    [
      {
        "userId": 1,
        "id": 1,
        "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
        "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
      },
```
 > to flatten to 1 array container can use concat.apply


```javascript
        getEntries() {
            var apiUrl = 'https://jsonplaceholder.typicode.com/posts';

            this.$http.get(apiUrl)
                .then((response) => {
                    this.apiPosts.push(response.body);
                    this.apiPosts = [].concat.apply([], this.apiPosts);
                    console.log('done');
                }
                    );
        },
```

## Use console-like html biding. good for debunging:
```html
        <pre>
            {{$data}}
        </pre>
```


## Add an item to an array - input bind from html input with v-model
 1. Create an empty array 'events'
 2. create an object 'event'
 
 ```javascript
     data: {
        events: [],
        newEvent: {name: '', description: '', date: ''},
    },
```
 3. bind with v-model input in html referencing the 'event' object
 
 ``` html
 <input type="text" class="form-control" placeholder="event name" v-model="event.name">
 <textarea type="text" class="form-control" placeholder="description" v-model="event.description"></textarea>
 <input type="date" class="form-control" placeholder="date" v-model="event.date">
 ```
 4. Add a function to methods to push the 'event' object to 'events' array
 
 ```javascript
  addEvent: function() {
     var event = this.newEvent;
     this.events.push(event);
 }
```

 5. Add 'addEvent()' to html button
 
```html
<button class=" btn btn-primary" v-on:click="addEvent()">add event</button>
```


## Get GitHub Issue with pagination indication (e.g rel = 'next')

```javascript
new Vue({
    el: '#app',

    data: {
        apiHeadersPageStatus: '',
        gitPaginationHeaderLink: '',
        gitBaseUrl: 'https://github.ibm.com/api/v3/repos/EMEA-Accelerate/Core-Team/issues?access_token=xxxxxxxxx&per_page=100&state=all&page=',
        gitIssueNumbersList: [],
        gitIssuesList: [],

    },

    ready: function() {
    },

    methods: {

        getGitIssues(i) {

            axios.get(this.gitBaseUrl + i)
                .then(response => {
                    this.gitIssuesList.push(response.data);
                    this.gitIssuesList = [].concat.apply([], this.gitIssuesList);
                    this.gitPaginationHeaderLink = response.headers.link;
                    this.apiHeadersPageStatus = this.gitPaginationHeaderLink.substring(this.gitPaginationHeaderLink.indexOf('rel=\"')+5, this.gitPaginationHeaderLink.lastIndexOf('\", <'));
                })
                .catch(error => 'Git get didn\'t work')
            console.log('done');

        }
    }
})
```

## Check box v-model false / true

```html
<input type="checkbox" name="" value="" class="checkbox" v-model="task.checked">
```

```javascript
new Vue({
  el: '#todo',
  data: {
    newTask: '',
    taskList: []
  },

  methods: {
    addTask () {
      let task = this.newTask.trim();

      if(task) {
        this.taskList.push({
          text: task,
          checked: false
        })
        this.newTask = '';
      }
    }
  }
})
```

## Remove an item from an array

```javascript
    removeTask (task) {
      let index = this.taskList.indexOf(task);
      this.taskList.splice(index, 1);
    }
```



## Webpack snipets

``` javascript
// loader for json file
      {
        test: /\.json$/,
        loader: 'json'
      },


// loader for scss files
      {
        test: /\.scss$/,
        loaders: ["style", "css", "sass"]
      },
      
// Include jquery

  plugins: [
    new webpack.ProvidePlugin({
      $: "jquery/dist/jquery",
      jQuery: "jquery/dist/jquery",
      "window.jQuery": "jquery/dist/jquery",
      "window.$": "jquery/dist/jquery"
    }),
  ],
      
```

