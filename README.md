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
  baseUrl: string = 'https://github.ibm.com/api/v3/repos';
  repoUrl: string = 'EMEA-Accelerate/Core-Team';
  itemTypeUrl: string = 'issues';
  tokenUrl: string = '?access_token=d6ce7c2a96a7146f1da65e4392c37d19db1a7a7f';

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


