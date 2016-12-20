#Vue js  Cook Book

## Create a Github repo from Local repository

Adding an existing project to GitHub using the command line
From <https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/> 

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

