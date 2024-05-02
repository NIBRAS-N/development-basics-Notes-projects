### DOcumentation: https://rxjs.dev/guide/overview

# Basic of observable:

```ts
import { Observable } from 'rxjs';

// Create an Observable that emits a sequence of numbers
const observable = new Observable<number>(observer => {
  observer.next(1); // Emit the first value
  observer.next(2); // Emit the second value
  observer.next(3); // Emit the third value
  observer.complete(); // Complete the Observable
});

// Subscribe to the Observable
const subscription = observable.subscribe({
  next: value => console.log('Value:', value), // Handle each emitted value
  error: err => console.error('Error:', err), // Handle errors
  complete: () => console.log('Observable completed') // Handle completion
});

```
# Pipe module and map + filter + tap operator with obervable:

```ts
import { Observable } from 'rxjs';
import { map, filter, tap } from 'rxjs/operators';

// Create an Observable that emits a sequence of numbers
const observable = new Observable<number>(observer => {
  observer.next(1); // Emit the first value
  observer.next(2); // Emit the second value
  observer.next(3); // Emit the third value
  observer.complete(); // Complete the Observable
});

// Apply the tap, map, and filter operators to log, transform, and filter the emitted values
const transformedObservable = observable.pipe(
  tap(value => console.log('Original value:', value)), // Log the original value
  map(value => value * 2), // Double each emitted value
  filter(value => value > 2) , // Filter values greater than 2
  shareReply()
);

// Subscribe to the transformed and filtered Observable
const subscription = transformedObservable.subscribe({
  next: value => console.log('Transformed and filtered value:', value), // Handle each transformed and filtered value
  error: err => console.error('Error:', err), // Handle errors
  complete: () => console.log('Observable completed') // Handle completion
});


```
### Observer + form event + api Handling:
  - Creates an Observable that emits events of a specific type coming from the given event target.
  - https://rxjs.dev/api/index/function/fromEvent
```ts

export class AppComponent{
  @ViewChild('search',{static:true})
  abc!:ElementRef<HTMLInputElement>

  typeAheadSub?:Subscription

  ngOnInit(){
    const searchObs = fromEvent(this.abc.!nativeELement,"input").pipe(
                            // after 1 sec api will start calling
                            debounceTime(1000),
                            filter((e:any)=>e.target.value != ""),
                            switchMap((e:any)=>{
                                return ajax(`https://api/githu.com/search/users?q=${e.target.value}`);
                            }),
                            map(ajaxResponse => ajaxResponse.response.items)
                      )

    this.typeAheadSub=searchObs.subscribe((val)=>console.log(val.target.value));
    
  }
  ngOnDestroy(){
    this.typeAheadSub.unsubscribe();
  }
}
```

# Multicasting:

- https://www.learnrxjs.io/learn-rxjs/operators/multicasting

# subject:

- https://rxjs.dev/guide/subject
