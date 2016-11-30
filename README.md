# Angular2-CookBook


+ Service that takes an array inside an object
	- For example you have a DB like this:
```json
{
  "total_rows": 2,
  "offset": 0,
  "rows": [
    {
      "id": "7ee196004d9b1c2e9e4c74a1d9b588a1",
      "key": "7ee196004d9b1c2e9e4c74a1d9b588a1",
      "value": {
        "rev": "1-967a00dff5e02add41819138abb3284d"
      }
    },
    {
      "id": "9d73a3ef260405a0fd702dcf385e9e4e",
      "key": "9d73a3ef260405a0fd702dcf385e9e4e",
      "value": {
        "rev": "1-68bf69a09284f2fc7c4d3ede47e59d01"
      }
    }
  ]
}
```
	- You want only to take what is inside rows array and exclude summary information (total_rows and offset)
	
```typescript
// service.ts

import { Injectable } from '@angular/core';
import { Http} from '@angular/http';
import { Observable } from 'rxjs/Observable';

@Injectable()
export class GetCloudantService {

  serviceUrl: string = 'https://96d7577e-6f81-4571-9849-417aebe1a7b6-bluemix.cloudant.com/my_sample_db/_all_docs';

  constructor(private http: Http) { }

  getEntries(): Observable<any> {
    return this.http.get(this.serviceUrl)
      .map((response) => response.json().rows);
  }

}

```

```typescript
// component.ts
import { Component, OnInit } from '@angular/core';
import { GetCloudantService } from '../services/get-cloudant.service';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit {

  items: [any];

  constructor(private getCloudantService: GetCloudantService) { }

  ngOnInit() {
    this.getCloudantService.getEntries()
      .subscribe(
        items => this.items = items,
        error => console.log(`Errror didn't work`)
      );
  }


}
```




+ Error: “No 'Access-Control-Allow-Origin' header is present on the requested resource”


up vote
243
down vote
This is not a fix for production or when application has to be shown to the client, this is only helpful when UI and Backend development are on different servers and in production they are actually on same server. For example: While developing UI for any application if there is a need to test it locally pointing it to backend server, in that scenario this is the perfect fix. For production fix, CORS headers has to be added to the backend server to allow cross origin access.
The easy way is to just add the extension in google chrome to allow access using CORS.

(https://chrome.google.com/webstore/detail/allow-control-allow-origi/nlfbmbojpeacfghkpbjhddihlkkiljbi?hl=en-US)

Just enable this extension whenever you want allow access to no 'access-control-allow-origin' header request.

Or

In Windows, paste this command in run window

chrome.exe --user-data-dir="C:/Chrome dev session" --disable-web-security
this will open a new chrome browser which allow access to no 'access-control-allow-origin' header request.

shareedit
edited Sep 14 at 4:41
answered Mar 4 '15 at 6:42

shruti
2,858189



+ VS Code, exclude specific file type:
  - In Vs code : File > Preferencies > Workspace setting. This will create a new folder with setting.json file on the root directory
  - under the project explorer open .vscode folder and then setting.json. This allows to change configuration of VS code for the specific project
  - add files.exclude:
  ```json
  {
    "typescript.tsdk": "./node_modules/typescript/lib",
	
    "files.exclude": {
		"**/.git": true,
		"**/.DS_Store": true,
		"**/.js": true,
		"**/.js.map": true,
		"**/*.spec.ts": true
		
	}    
}
  ```

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

+ Combine multiple http call in 1 single array (hard coded instead of looping trough):
  - forkJoin 2 or more http call 
  - subscribe to combine result 
  - redue combined result to flatten the array of arrays

service.ts
```typescript
  getAllGitHubIssues(): Observable<any> {
    return Observable.forkJoin(
      this.http.get(`${this.gitFullUrl}&state=all&page=1`).map((response) => response.json()),
      this.http.get(`${this.gitFullUrl}&state=all&page=2`).map((response) => response.json()),
      this.http.get(`${this.gitFullUrl}&state=all&page=3`).map((response) => response.json())
    );
  }
```

app.ts
```typescript
  ngOnInit() {
    this.gitHubApiService.getAllGitHubIssues()
      .subscribe(
        item => this.allGitIssues = [item[0], item[1], item[2]].reduce((a, b) => a.concat(b)),
        error => console.log('I am an errorr from dashboard'),
        () => console.log(`I am done I gave you ${this.allGitIssues.length} issues. Here is what I get: ${this.allGitIssues}`)
      );
  }

```

+ Do more actions during .subscribe() asyncronupsly
```typescript
  getAllGitHubIssues() {
    this.gitHubApiService.getAllGitHubIssues()
    .subscribe(
      item => {
        this.allGitIssues = [item[0], item[1], item[2]].reduce((a, b) => a.concat(b));

        this.totalCount = this.allGitIssues.length;
        this.openCount = this.allGitIssues.filter(x => x.state === 'open').length;
        this.closedCount = this.allGitIssues.filter(x => x.state === 'closed').length;
        this.closedPerc = (this.closedCount / this.totalCount) * 100;
        this.openPerc = (this.openCount / this.totalCount) * 100;
        this.daysOpen = this.allGitIssues.closed_at - this.allGitIssues.created_at;
      },
      error => console.log('I am an errorr from dashboard'),
      () => console.log(`I am dashboard, done I gave you ${this.allGitIssues.length} issues.}`)
    );
}
```

