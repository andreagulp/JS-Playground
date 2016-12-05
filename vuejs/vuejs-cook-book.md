#Vue js  Cook Book

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
          <pre>
              {{$data | json}}
          </pre>
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
 > to flatten to 1 array container
```javascript
        getEntries() {
            var apiUrl = 'https://jsonplaceholder.typicode.com/posts';

            this.$http.get(apiUrl)
                .then((response) => {
                    this.apiPosts.push(response.body);
                    // this.apiPosts = [].concat.apply([], this.apiPosts);
                    console.log('done');
                }
                    );
        },
```

