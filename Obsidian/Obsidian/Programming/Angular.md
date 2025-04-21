
# Components Lecture

- Component decorator with 
```typescript
@Component({
	selector: "app-nav",
	template: `<div id="nav-wrapper">{{title}}</div>`
	// or
	templateUrl: "./navigation.component.html"
	styles: ["#nav-wrapper: {background-color: pink}"]
	// or
	styleUrls: ['./navigation.component.css']
})
export class NavigationComponent {
}
```

- after the creation of a component, we need to add it in the declarations array at the app module
- NgModules help organize an application into cohesive blocks of functionality

```typescript
@NgModule({
	declarations: [
	AppComponent,
	NavComponent,
	],
	imports: [
	BrowserModule,
	AppRoutingModule
	],
	providers: [],
	bootstrap: [AppComponent]
})
export class AppModule {}
```

### \*ngFor=
```typescript
export class NavigationComponent {

    public navigationLis: string[] = ["Home", "About", "Register", "Login"]
}
```
```html
<nav>
    <ul>
        <li *ngFor="let liContent of navigationLis">{{liContent}}</li>
    </ul>
</nav>
```

### \*ngIf=
```typescript
export class PlaygroundComponent implements OnInit, OnDestroy {
  isToggle: boolean = false;
}
```
```html
<div *ngIf="isToggle">
    <p>Toggle Element!</p>
</div>
```

### Attach Events
```html
<button (click)="handleClick($event)">Click Me!</button>
```
```typescript
  handleClick(event: Event): void {
    console.log(event.target);
    this.isToggle = !this.isToggle;
  }
```

### Binding Attributes
```typescript
export class NavComponent {
	imgUrl: string = "https://...";

	dynamicClass: string = "active";

	isHiddenDivSection: boolean = true;
}
```
```html
	<img [attr.src]="imgUrl"/>

	<div [class]="dynamicClass">
	</div>

	<div [class.hidden]="isHiddenDivSection">
	</div>

	<button [style.color]="isHiddenDivSection">
		Click me!
	</button>

	<button [style.font-size.rem]="!isHiddenDivSection ? 3 : 1">
		Click me too!
	</button>

```

### Angular *UseRef*
```html
<input #phone placeholder="phone number"/>

<button (click)="callPhone(phone.value)">
	Call
</button>
```

### Angular *UseEffect* with empty dependency array
```typescript
...
export class GamesComponent implements OnInit {
	ngOnInit() {
		console.log("Mounted Component");
	}
}
```

### On Component Destruction
```typescript
export class GamesComponent implements onDestroy {
	ngOnDestroy() {
		console.log("Component Unmounted");
	}
}
```

### Component Interaction

#### From Parent to Child
```html
	<app-playground color="darkred"/>
```
```typescript
export class PlaygroundComponent {
	@Input("color") colorValue = "white";
	...
}
```
```html
<button [style.background-color]="colorValue">
	Click Me!
</button>
```


#### From Child to Parent
```typescript
export class PlaygroundComponent {
	@Output onTestOutput = new EventEmitter<string>();

	handleInput(username: string): void {
		this.onTestOutput.emit(username);
	}
}
```
```html
<input type="text" #username/>
<button (click)="handleInput(username.value)">
	Handle Input Button
</button>
```
```html
<app-playground (onTestOutput)="onOutputFromChild($event)"/>
```
```typescript
export class AppComponent {
	onOutputFromChild(username: string): void {
		console.log("Data received in App Parent:", username);
	}
}
```
