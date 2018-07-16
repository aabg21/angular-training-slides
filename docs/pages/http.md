<!-- .slide: data-background="../images/title-slide.jpg" -->
<!-- .slide: id="http" -->
## Building Applications with Angular

# The HttpClient Service

---
<!-- .slide: id="http-http-service" -->
## The Built-in `HttpClient` Service

- Service provided by Angular to perform REST operations
- Has a method for each HTTP verb: `get`, `post`, `put`, `delete`
- Each method returns an `Observable` that emits a single value
- Connections are closed automatically after the value is emitted

---
<!-- .slide: id="http-importing" -->
## Importing the Service

- We need to import `HttpClientModule` in `app.module.ts`

```ts
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  //...
  imports: [
    //...
    HttpClientModule
  ],
  //...
})
export class AppModule {}
```

---

<!-- .slide: id="http-fetching-data-1" -->
## Fetch Data From the Server

#### _src/app/todo.service.ts_
```ts
export class TodoService {

  baseUrl: string = 'http://localhost:3000';

  initialize() {
    const url = `${this.baseUrl}/items`;
    this.http
      .get(url)
      .map(res => res.json())
      .subscribe(
        (body) => {
          this.items = body.map(item => item.text);
        },
        (err) => { console.log(err); }
      );
  }
}
```

---

<!-- .slide: id="http-sending-data" -->
## Send Data to Server

#### _src/app/to-do.service.ts_
```ts
  addItem(item: string) {
    const url = `${this.baseUrl}/items`;
    this.http
      .post(url, {text: item})
      .map(res => res.json())
      .subscribe(
        (res) => {
          if (res) this.items.push(res);
        },
        (err) => { console.log(err); }
      );
  }
```

---

<!-- .slide: id="http-advanced-cancel-request" -->
## Cancel a Request

- One of the greatest benefits to using observables over promises is the ability to cancel `http` requests

```ts
  search() {
    const request = this.searchService.search(this.searchField.value)
      .subscribe(
        result => { this.result = result.artists.items; },
        err => { this.errorMessage = err.message; },
        () => { console.log('Completed'); }
      );

    // cancel request
    request.unsubscribe();
  }
```

---

<!-- .slide: id="http-advanced-retry-request" -->
## Retry a request

- Retry a failed request with the `retry` operator
- Useful for when a user's connection is lost
- `retry` takes an argument to specify the number of retries
- if no argument is given, the request will be retried indefinitely

```ts
  search(term: string) {
    let tryCount = 0;
    return this.http.get('https://api.spotify.com/v1/dsds?q=' + term + '&type=artist')
      .map(response => response.json())
      .retry(3);  // Will retry failed request 3 times
}
```
- **Note:** The `onError` callback will not execute during the retry phase. The stream will only throw an error after the retry phase is complete
