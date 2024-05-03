### DOcumentation: https://rxjs.dev/guide/overview

# Use Case:

![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/5d476dbd-34f7-4b3c-aa5a-49608913a6c3)


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

# HTTPCLient

![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/a5a70aa9-4edf-44f6-b746-f5336ad616a3)
![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/680ca301-87f2-43c6-9656-96ea706af790)



# Multicasting:

- https://www.learnrxjs.io/learn-rxjs/operators/multicasting

# subject:

- https://rxjs.dev/guide/subject

# Commonly used RxJS features with Reactive Forms:

- ### ValueChanges:
  
    - The valueChanges property of a form control or form group returns an  `observable`   that emits the current value of the form control or form group whenever it changes.
      
- ### DebounceTime:
  
     - The debounceTime operator delays the emission of values from an observable by a specified amount of time. It is often used with valueChanges to debounce user input and reduce unnecessary API calls or validation checks.
       
- ### DistinctUntilChanged:
  
  - The distinctUntilChanged operator filters out consecutive duplicate values emitted by an observable. It is useful when you want to prevent duplicate values from triggering unnecessary actions or updates.


```ts
import { Component } from '@angular/core';
import { FormControl } from '@angular/forms';
import { debounceTime, distinctUntilChanged } from 'rxjs/operators';

@Component({
  selector: 'app-search',
  imports: [RouterOutlet,UserProfileComponent,CommonModule, ReactiveFormsModule,],
  template: `
    <input type="text" [formControl]="searchControl" placeholder="Search">
  `
})
export class SearchComponent {
  searchControl = new FormControl('');

  constructor() {
    this.searchControl.valueChanges
      .pipe(
        debounceTime(300),
        distinctUntilChanged()
      )
      .subscribe(searchTerm => {
        // Perform desired action or API call with the debounced and distinct search term
        console.log(searchTerm);
      });
  }
}
```

- ### SwitchMap:
  - The switchMap operator is used for scenarios where you need to cancel previous inner observables and switch to a new observable when the source observable emits a new value. It is commonly used for making API calls based on user input.
 
```ts
@Component({
  selector: 'app-search',
  template: `
    <input type="text" [formControl]="searchControl" placeholder="Search">
  `
})
export class SearchComponent {
  searchControl = new FormControl('');

  constructor(private http: HttpClient) {
    this.searchControl.valueChanges
      .pipe(
        debounceTime(300),
        distinctUntilChanged(),
        switchMap(searchTerm => this.getSearchResults(searchTerm))
      )
      .subscribe(results => {
        // Handle the search results
        console.log(results);
      });
  }

  getSearchResults(searchTerm: string) {
    return this.http.get<any[]>('api/search', { params: { query: searchTerm } });
  }
}

```
- ### Merge:
  
  - The merge operator is used to combine multiple Observables into a single stream.
  - It can be useful when you need to react to changes from multiple form controls simultaneously.
  - For example, you might have two form fields where the validity of one depends on the value of the other. By merging the valueChanges Observables of both fields, you can perform validation or update dependent fields whenever either of them changes.
```ts
import { Component } from '@angular/core';
import { FormGroup, FormControl } from '@angular/forms';
import { merge } from 'rxjs';

@Component({
  selector: 'app-merge-example',
  template: `
    <form [formGroup]="myForm">
      <input formControlName="firstName" placeholder="First Name">
      <input formControlName="lastName" placeholder="Last Name">
      <button [disabled]="myForm.invalid">Submit</button>
    </form>
  `,
})
export class MergeExampleComponent {
  myForm: FormGroup;

  constructor() {
    this.myForm = new FormGroup({
      firstName: new FormControl(''),
      lastName: new FormControl(''),
    });

    merge(
      this.myForm.get('firstName').valueChanges,
      this.myForm.get('lastName').valueChanges
    ).subscribe(() => {
      console.log('Form value changed!');
      // Perform validation or update dependent fields here
    });
  }
}

```
- ### Catch Error:
  
  - The catchError operator is used for error handling in reactive scenarios.
  - When performing asynchronous operations like submitting a form or making API requests, you can use catchError to gracefully handle any errors that occur during these operations.
  - This allows you to display appropriate error messages or trigger alternative actions in case of failures.

```ts
import { Component } from '@angular/core';
import { FormGroup, FormControl } from '@angular/forms';
import { catchError } from 'rxjs/operators';
import { throwError } from 'rxjs';

@Component({
  selector: 'app-catch-error-example',
  template: `
    <form [formGroup]="myForm">
      <input formControlName="username" placeholder="Username">
      <button (click)="submitForm()">Submit</button>
    </form>
    <p *ngIf="errorMessage">{{ errorMessage }}</p>
  `,
})
export class CatchErrorExampleComponent {
  myForm: FormGroup;
  errorMessage: string;

  constructor() {
    this.myForm = new FormGroup({
      username: new FormControl(''),
    });
  }

  submitForm() {
    const username = this.myForm.get('username').value;

    // Simulating an API request that might fail
    performApiRequest(username).pipe(
      catchError((error) => {
        this.errorMessage = 'An error occurred. Please try again.';
        return throwError(error);
      })
    ).subscribe((response) => {
      // Handle successful response here
      console.log('API response:', response);
    });
  }
}

function performApiRequest(username: string) {
  // Simulating an API request that might fail
  if (username === 'admin') {
    return throwError('Invalid username');
  } else {
    return of('Success');
  }
}
```
- ### Combine Latest
  
  - The combineLatest operator combines the latest values from multiple Observables into a new Observable.
  - This is useful when you want to react to changes in multiple form controls simultaneously.
  - For example, you might need to enable or disable a form button based on the validity of multiple fields.
  - By using combineLatest, you can monitor the validity of all relevant form controls and update the button state accordingly.
    
```ts
import { Component } from '@angular/core';
import { FormGroup, FormControl } from '@angular/forms';
import { combineLatest } from 'rxjs';

@Component({
  selector: 'app-combine-latest-example',
  template: `
    <form [formGroup]="myForm">
      <input formControlName="firstName" placeholder="First Name">
      <input formControlName="lastName" placeholder="Last Name">
      <p>Full Name: {{ fullName }}</p>
    </form>
  `,
})
export class CombineLatestExampleComponent {
  myForm: FormGroup;
  fullName: string;

  constructor() {
    this.myForm = new FormGroup({
      firstName: new FormControl(''),
      lastName: new FormControl(''),
    });

    combineLatest([
      this.myForm.get('firstName').valueChanges,
      this.myForm.get('lastName').valueChanges
    ]).subscribe(([firstName, lastName]) => {
      this.fullName = firstName + ' ' + lastName;
    });
  }
}

```
