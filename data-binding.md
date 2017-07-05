## Data binding

Controller:
```
@Component({
  // selector: '[app-servers]',
  // selector: '.app-servers',
  selector: 'app-servers',
  // template: `
  //   <app-server></app-server>
  //   <app-server></app-server>`,
  templateUrl: './servers.component.html',
  styleUrls: ['./servers.component.css']
})
export class ServersComponent implements OnInit {
  allowNewServer = false;
  serverCreationStatus = 'No server was created!';
  serverName = 'Testserver';

  constructor() {
    setTimeout(() => {
      this.allowNewServer = true;
    }, 2000);
  }

  ngOnInit() {
  }

  onCreateServer() {
    this.serverCreationStatus = 'Server was created! Name is ' + this.serverName;
  }

  onUpdateServerName(event: Event) {
    this.serverName = (<HTMLInputElement>event.target).value;
  }
}
```
**String interpolation** 
```
<p>{{ 'Server' }} with ID {{ serverId }} is {{ getServerStatus() }}</p>
```
**Property binding**
HTML:
```
<button
  class="btn btn-primary"
  [disabled]="!allowNewServer"
  (click)="onCreateServer()">Add Server</button>
```

**Event binding**
HTML:
```
<button
  class="btn btn-primary"
  [disabled]="!allowNewServer"
  (click)="onCreateServer()">Add Server</button>
```

Controller:
```
 onCreateServer() {
    this.serverCreationStatus = 'Server was created! Name is ' + this.serverName;
  }
```

**Events with properties:**

HTML:
```
<input-->
  <!--type="text"-->
  <!--class="form-control"-->
  <!--(input)="onUpdateServerName($event)">
```

Controller:
```
onUpdateServerName(event: Event) {
    this.serverName = (<HTMLInputElement>event.target).value;
  }
```

**Two-way binding**

```
<input
  type="text"
  class="form-control"
  [(ngModel)]="serverName">
  ```
