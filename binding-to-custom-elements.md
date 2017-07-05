## Binding to custom elements

**Input binding**

HTML:

```
<div class="row">
  <div class="col-xs-12">
    <app-recipe-item
      *ngFor="let recipeEl of recipes; let i = index"
      [recipe]="recipeEl"
      [index]="i"></app-recipe-item>
  </div>
</div>

```

Controller app-recipe-item

```

@Component({
  selector: 'app-recipe-item',
  templateUrl: './recipe-item.component.html',
  styleUrls: ['./recipe-item.component.css']
})
export class RecipeItemComponent implements OnInit {
  @Input() recipe: Recipe;
  @Input() index: number;

  ngOnInit() {
  }
}
```
Aliases:
```
 @Input("foodRecipe") recipe: Recipe;
...........................................................

 <div class="row">
  <div class="col-xs-12">
    <app-recipe-item
      *ngFor="let recipeEl of recipes; let i = index"
      [foodRecipe]="recipeEl"
      [index]="i"></app-recipe-item>
  </div>
</div>
```
**Output event binding**

Controller:
```
@Component({
  selector: 'app-cockpit',
  templateUrl: './cockpit.component.html',
  styleUrls: ['./cockpit.component.css']
})
export class CockpitComponent implements OnInit {
  @Output() serverCreated = new EventEmitter<{serverName: string, serverContent: string}>();
  @Output('bpCreated') blueprintCreated = new EventEmitter<{serverName: string, serverContent: string}>();
  // newServerName = '';
  // newServerContent = '';
  @ViewChild('serverContentInput') serverContentInput: ElementRef;

  constructor() { }

  ngOnInit() {
  }

  onAddServer(nameInput: HTMLInputElement) {
    this.serverCreated.emit({
      serverName: nameInput.value,
      serverContent: this.serverContentInput.nativeElement.value
    });
  }

  onAddBlueprint(nameInput: HTMLInputElement) {
    this.blueprintCreated.emit({
      serverName: nameInput.value,
      serverContent: this.serverContentInput.nativeElement.value
    });
  }

}

............................................................................


  onServerAdded(serverData: {serverName: string, serverContent: string}) {
    this.serverElements.push({
      type: 'server',
      name: serverData.serverName,
      content: serverData.serverContent
    });
  }

  onBlueprintAdded(blueprintData: {serverName: string, serverContent: string}) {
    this.serverElements.push({
      type: 'blueprint',
      name: blueprintData.serverName,
      content: blueprintData.serverContent
    });
  }

..................................................................................................

<div class="container">
  <app-cockpit
    (serverCreated)="onServerAdded($event)"
    (bpCreated)="onBlueprintAdded($event)"
  ></app-cockpit>
  <hr>
  ...
  
  ```
also output bindings can have aliases. Another example is although irelevant

**Reference binding**

reference element can be used only in the component template, not in ts code

```

  <input
      type="text"
      class="form-control"
      #serverNameInput>
      
    <button
      class="btn btn-primary"
      (click)="onAddServer(serverNameInput)">Add Server</button>
      
 ...................................................................................................
 
 onAddServer(nameInput: HTMLInputElement) {
    this.serverCreated.emit({
      serverName: nameInput.value,
      serverContent: this.serverContentInput.nativeElement.value
    });
  }
 
```

**Inject view element into the controller**

```
@ViewChild('serverContentInput') serverContentInput: ElementRef;
       
...

onAddServer(nameInput: HTMLInputElement) {
    this.serverCreated.emit({
      serverName: nameInput.value,
      serverContent: this.serverContentInput.nativeElement.value
    });
  }
```
**ng-content is similar to ng-transclude from angularJs**

```
<div
  class="panel panel-default">
  <!--<div class="panel-heading">{{ element.name }}</div>-->
  <div class="panel-heading" #heading>{{ name }}</div>
  <div class="panel-body">
    <ng-content></ng-content>
  </div>
</div>

............................................................

 <app-server-element
        *ngFor="let serverElement of serverElements"
        [srvElement]="serverElement"
        [name]="serverElement.name">
        
        //transclusion
        <p #contentParagraph>
          <strong *ngIf="serverElement.type === 'server'" style="color: red">{{ serverElement.content }}</strong>
          <em *ngIf="serverElement.type === 'blueprint'">{{ serverElement.content }}</em>
        </p>
        //end transclusion
      </app-server-element>
      
```
