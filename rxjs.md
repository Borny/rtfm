# RXJS - Observables

- Overview
- Subject
- BehaviorSubject
- Unsubscribe
- map
- tap
- exhaustMap
- take
- catchError - throwError
- switchMap
- retry
- mapTo
- merge


## Overview

**Subscribe** to an **Observable** is the equivalent of **calling** a **function**.  
An Observable alone won't do anythind unless an **observer** subscribes to it.

Observable - subscribe - Observer

Observer is the **argument** of the **subscribe** method.

**Observer** is a simple function that will use the value returned by the **Observable**

## Subject

It allows you to pass data accross the entire app. 
Create a subject in a service and pass it some data. This data will then be available to any component that subscribes to it.

**Service/Directive :**
```javascript
import {Subject} from 'rxjs';

private subjectCustomName: Subject<any> = new Subject<any>();

method(){
  this.subjectCustomName.next(someData);
}
```

**Component :**
```javascript

this.property: any;

method(){
  this.serviceName.subjectCustomName()
  .pipe(
    takeUntil(this.destroy$) // necessary to unsubscribe from the observable in the ngOnDestroy method
  )
  .subscribe((data) => {
    this.property = data;
  })
}
```
## BehaviorSubject

It allows you to pass data accross the entire app with an initial value. 
Create a BehaviorSubject in a service and pass it some data. This data will then be available to any component that subscribes to it.

**Service/Directive :**
```javascript
import {BehaviorSubject} from 'rxjs';

private behaviorSubjectCustomName$: BehaviorSubject<any> = new BehaviorSubject<any>(initialData);

method(){
  this.subjectCustomName.next(someData);
}
```

**Component :**
```javascript

this.property: any;

method(){
  this.serviceName.subjectCustomName()
  .pipe(
    takeUntil(this.destroy$) // necessary to unsubscribe from the observable in the ngOnDestroy method
  )
  .subscribe((data) => {
    this.property = data;
  })
}
```

## Unsubscribe
When creating our own Observable, it is necessary to **unsubscribe** to avoid memory leaks.

```javascript
import { takeUntil } from 'rxjs/operators';
import { Subject } from 'rxjs';

export class PostListComponent implements OnInit, OnDestroy {
  public posts: Post[] = [];

  private destroy$: Subject<any> = new Subject();

  constructor(private postsService: PostsService) { }

  ngOnInit() {
    this.posts = this.postsService.getPosts();
    this.postsService.getPostUpdateListener()
      .pipe(
        takeUntil(this.destroy$)
      )
      .subscribe((posts: Post[]) => {
        this.posts = posts;
      });
  }

  ngOnDestroy(): void {
    this.destroy$.next();
    this.destroy$.unsubscribe();
  }
}

```

## map

Will take the response and transform it. Needs to return a value

```javascript
map(response => {
    console.log(response);
    return response;
  }),
```

## tap

Will take the response without transforming it

## exhaustMap

...

## take

Will take the number of response desired.  
e.g:
`take(2)` // will only care about the first two responses and will then complete

## catchError - throwError

Will catch the potential error returned by an Http request.

```javascript
catchError(error => {
  console.log(error);
  return of(error);
});
```

## switchMap

Waits for an observable to complete then use the data from that observable and returns a new observable.

## retry

Will resubscribe a defined amount of time. i.e: in the case of a http call, we can try to make another call twice:
```javascript
getData(): Observable<Data[]>{
  return this.httpClient.get<Data[]>(this.apiUrl)
  .pipe(
    retry(2), // retry a failed request up to 2 times
    catchError(this.handleError<Data>('getData'))
  );
}
```

## mapTo

Will convert any value in to whatever value we pass as an argument.

## merge

Will combine two or more observable into one observable. It will emit only one value at a time.