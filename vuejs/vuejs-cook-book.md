#Vue js  Cook Book

## Intialize basic js project with npm
 1. Create Project folder
 2. Gitbash the folder
 3. `npm init`
 4. `npm install vue vue-resource bootstrap`
 5. `touch index.html app.js`

## Generate basic HTML boilerplate with emmet
```html
! <tab>
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
        event: {name: '', description: '', date: ''},
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
     this.events.push(this.event);
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
