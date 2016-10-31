# Angular2-CookBook


+ Create an array with 30 items (progressing number from 0 to 29.
  Good for testing, can create random array
```typescript
  //ts.
  constructor() {
    this.items = Array(30).map(i => i);
  }
```

+ Call GitHub API, HTTP and Observable

```html
  <!--component.html.-->
    <li *ngFor="let item of items | slice:0:10" class="post">
      {{item.title}}
    </li>
```

```typescript
//component .ts

  ngOnInit() {
    this._hackernewsApiService.fetchStories()
      .subscribe(
        items  => this.items = items,
        error => console.log(`Errror didn't work`));
 }
```


```typescript
//services .ts

@Injectable()
export class HackernewsApiService {
  baseUrl: string = 'https://github.xxx.com/api/v3/repos';
  repoUrl: string = 'xxx-xxx/xxx-xxx';
  itemTypeUrl: string = 'issues';
  tokenUrl: string = '?access_token=xxxxxxxxxxxxxxx';

  constructor(private http: Http) {}

  fetchStories(): Observable<any> {
    return this.http.get(`${this.baseUrl}/${this.repoUrl}/${this.itemTypeUrl}${this.tokenUrl}`)
      .map(response => response.json()
      )
  }
  ```
  
  
+ Take only the n items in a list
```typescript
```  

```html
    <li *ngFor="let item of items | slice:0:10" class="post">
      {{item.title}}
    </li>
```

+ Dependent calls: 
from syntaxsuccess
Another common scenario is a call sequence of dependent http calls. In the below example I will make an initial call to load a customer. The returned customer object contains a contract url that I will be using to load a contract for that particular customer.

```typescript
this.http.get('./customer.json').map((res: Response) => {
               this.customer = res.json();
               return this.customer;
            })
            .flatMap((customer) => this.http.get(customer.contractUrl)).map((res: Response) => res.json())
            .subscribe(res => this.contract = res);
```  


+ Dependent calls:
  - get git issues
  - pass issue.number to component with @Input
  - get zenhub issue
  - initiate zenHub issue on the component receiving the input
  - from https://github.com/hdjirdeh/angular2-hn

+ Observable filter operator (notice how filter is nested inside .map)
```typescript
  getClosedIssues(): Observable<any> {
    return this.http.get(this.gitApiUrl)
      .map(response => response.json()
      .filter(x => x.state === 'closed')
      );
  }
```  
 
 + Generate with RANGE number from 1 to 50, then transform toString, filter out '10' and then return a string that contanates all value separated by the symbol '|'. (Notice how filter is not nested in to the map operator like in the example above)
```typescript
   genIssues(){
    let obs = Observable
                .range(1, 50)
                .map(x => x.toString())
                .filter(x => x !== '10')
                .reduce((acc, cur) => acc + ' | ' + cur);
    return obs;
  }
```  
+ Assign local variable in the template

app.html
```html
      <table style="width: 100%" cellspacing="0"><tr>
        <td><md-input placeholder="link" style="width: 100%" #newLink></md-input></td>
      </tr></table>
      
      <button md-fab (click)="addArticle(newTitle, newLink)" >
        <md-icon class="md-24">add</md-icon>
      </button>
```  

app.ts
```typescript
  addArticle(title: HTMLInputElement, link: HTMLInputElement) {
    console.log(`I'm printing the title: ${title.value} and the link: ${link.value}`);
    return false;
  }
```  
```typescript
```  

+ How to sort a list

app.ts
```typescript
export class ArticleFormComponent implements OnInit {
  articles: Article[]

  constructor() {
    this.articles = [
      new Article('Webpack update', 'https://github.com', 5),
      new Article('Prerequisites', 'https://freter.io', 5),
      new Article('Table of Contents', 'https://nmnmnmnm.tn', 5)
    ]
   }

  ngOnInit() {
  }

  addArticle(title: HTMLInputElement, link: HTMLInputElement) {
    console.log(`I'm printing the title: ${title.value} and the link: ${link.value}`);
    this.articles.push(new Article(title.value, link.value, 0));
    title.value  = '';
    link.value = '';
    return false;
  }

  sortedArticles(): Article[] {
    return this.articles.sort((a, b) => b.votes - a.votes);
  }

}
```  

app.html have the ngFor loop trough sortedarticle() rather than articles[]

```html
<md-card *ngFor="let article of sortedArticles()">
<app-article [article] = "article" ></app-article>
</md-card>
```

+ use bootstrap only (no ng-version of it)

```html
<head>
  <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
</head>
```


