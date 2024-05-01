# Documentation Link:

- https://angular.dev/tools/cli/setup-local

# Set up procedure:

- npm install -g @angular/cli
- ng version
- npm run start
- ng new my-app
  
# In app create folder name `components` and file name `user-profile`:

- ng g c components/user-profile


# topics:

- ### ***Interpolation:***
    - {{}}

- ### ***Property Binding:***

    - in .ts
    ```TS
    export class UserProfileComponent {
  
        name = "nibras"

        home = "ctg"

        status = true

    }
    ```
    - in .html
    ```Ts
    <button [disabled]="status"> CLick me </button>
    ```

- ### ***Event Binding and 2 way data binding:***
    `-  Way-1:`
    - **Function needed.**
    - in .ts
    ```TS
    export class UserProfileComponent {
  
        name = "nibras"

        home = "ctg"

        status = true

        cd = ""
        abc = (e:Event) => {
            this.cd = (e.target as      HTMLInputElement).value;
            console.log(this.cd);
        }
    }
    ```
    - In .html
    ```TS
        <p>{{cd}}</p>
        <input type="text" [value]="cd" (input)="abc($event)" placeholder="Input Your idea"/>
    ```
    `- Way 2:`
    - **No need of function**
    - First add `FormsModule` in Component -> `imports: [FormsModule]` in .ts file. 

    - In .html
    ```Ts
        <input type="text" [(ngModel)]="cd"/>
    ```

- ### ***Control flow (@for @if @else)***

    - In .ts
    ```ts
    export class UserProfileComponent {
  
  
 
        users = [
            { id: 1, name: "John Doe", email: "john@example.com" },
            { id: 2, name: "Jane Smith", email: "jane@example.com" },
            { id: 3, name: "Alice Johnson", email: "alice@example.com" },
            { id: 4, name: "Bob Brown", email: "bob@example.com" },
            { id: 5, name: "Emily Davis", email: "emily@example.com" }
        ];
    }

    ```

    - In .html:
    ```ts

    <div>
        @for (item of users; track item) {
            @if(item.id == 1){

                <h1>{{item.email}} - {{item.name}} - {{item.id}}</h1>
            }@else {
                <h4>{{item.email}} - {{item.name}} - {{item.id}}</h4>
            }
        }

        
    </div>
    ```
- ### ***Directives:***

    - First add `FormsModule` in Component -> `imports: [CommonModule]
    - In .htmls file:

    ```ts
    <div *ngFor="let item of users">
        <h1 *ngIf="item.id==1;else elsePart">{{item.name}} - {{item.id}}</h1>

        <ng-template #elsePart>
            <h4 >{{item.name}} - {{item.id}}</h4>
        </ng-template>
        
        
    </div>
    
    
    ```

- ### ***Input Decorators : Parent to child data transfer***

    -  in parent .html file:
    ```ts
    <app-user-profile *ngFor="let item of users" [myName]="item.name" [myName2]="item.email" [age]="item.id" />
    <app-user-profile myName="abc" myName2="abc2"  myName3="abc3"  />
    <app-user-profile myName="def" myName2="def2" myName3="def3" />
    <app-user-profile myName="ghi"myName2="ghi2" myName3="ghi3" />

    ```

    - In child .ts file:

    ```ts
        export class UserProfileComponent {
  
            @Input() myName=""
            @Input({transform:numberAttribute}) age!:number
            @Input() myName2=""
            @Input({alias:"myName3",transform:addingHi}) user=""
        }
    ```

    - Now u can access` myName` and `myName2` and `user` in child .html file


- ### ***Output Decorator: Pass data from child to parents:***
    - `Example1`
    - in child .html 
    ```ts
    
    <button (click)="changeData()"> Click here</button>

    ```

    - in child .ts 

    ```ts
    export class UserProfileComponent {
  
        
        @Output() myEvent = new EventEmitter<string>();

        changeData(){
            this.myEvent.emit("DOnt quit")
        }
    }
    ```

    - in parent.html

    ```ts
    <app-user-profile 
        (myEvent)="receivedData($event)"
    />
    ```

    - in parent .ts

    ```ts
         receivedData(e:string){
            console.log(e);
        }
    ```
    - `Example-2: complex one`

    - in child .html
    ```ts
            <button (click)="changeData()"> Click here</button>
    ```
    - in child .ts
    ```ts
    @Output() myEvent = new EventEmitter<user>();

    changeData(){
        this.myEvent.emit({name:"dj",role:"djk",age:500})
    }
    ```

    - in parent .html

    ```ts
    <app-user-profile 
       
        (myEvent)="receivedData($event)"
    />
    ```

    - in parent .ts

    ```ts
        receivedData(e:user){
            console.log(e);

        } 
    ```

- ### ***BUILD IN PIPES***:

- Follow this Link: 
        -https://codecraft.tv/courses/angular/pipes/built-in-pipes/

- ### ***Custom Pipes***:

    - ng g p pipes/countryCode
    - in countryCode.pipe.ts file, there will be `transform function`. return from there what is necessary.
    - add `imports: [FormsModule,CommonModule,CountryCodePipe]` in  user .ts file
    ```ts
    export class CountryCodePipe implements PipeTransform {

    transform(value: unknown, ...args: unknown[]): unknown {
    return '+88'+value;
    }
    }
    ```

- ### Custom directive: @HostBinding @HostListener:
    - ng g d directives/highlights
    - add this in component .ts file:
        - `imports: [FormsModule,CommonModule,CountryCodePipe,HighlightDirective],`
    - in highlights .ts file:
    -
    ```ts
    import { Directive, ElementRef, HostBinding, HostListener } from '@angular/core';

    @Directive({
    selector: '[appHighlight]',
    standalone: true
    })

    export class HighlightDirective {

  constructor(private el:ElementRef) {
   }

   @HostBinding("style.backgroundColor")
   bgColor = "red"

   @HostListener("mouseenter")
   changeFontSize(){
    this.el.nativeElement.style.fontSize = "50px"
   }
  
   @HostListener("mouseleave")
   resetFontSize(){
    this.el.nativeElement.style.fontSize = "30px"
   }
  

    }
    ```
    - in component .html file:
    ```ts
    <h1 appHighlight></h1>
    ```
- ###  **Life Cycle Method:**
    - constructor()
    - ngOnChanges()
    - ngOnInit()
    - ngOnDestroy()
    - ngAfterViewInit()
 
- ### **Routing:**
    - Documentation Link:
        - https://angular.dev/guide/routing
     
- ### **Form**
    - Reactive forms
    - template-driven form

- ### **LazyLoading**
    - https://angular.io/guide/lazy-loading-ngmodules
- ### **Deffarable views**
