## Routing


**Routing definition:**

```javascript
const appRoutes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'users', component: UsersComponent, children: [
    { path: ':id/:name', component: UserComponent }
  ] },
  {
    path: 'servers',
    // canActivate: [AuthGuard],
    canActivateChild: [AuthGuard],
    component: ServersComponent,
    children: [
    { path: ':id', component: ServerComponent, resolve: {server: ServerResolver} },
    { path: ':id/edit', component: EditServerComponent, canDeactivate: [CanDeactivateGuard] }
  ] },
  // { path: 'not-found', component: PageNotFoundComponent },
  { path: 'not-found', component: ErrorPageComponent, data: {message: 'Page not found!'} },
  { path: '**', redirectTo: '/not-found' }
];

@NgModule({
  imports: [
    // RouterModule.forRoot(appRoutes, {useHash: true})
    RouterModule.forRoot(appRoutes)
  ],
  exports: [RouterModule]
})
export class AppRoutingModule {

}
```

```HTML
<div class="row">
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <router-outlet></router-outlet>
    </div>
  </div>
 ```
**Directives for navigation**


```HTML
<li role="presentation"
            routerLinkActive="active"
            [routerLinkActiveOptions]="{exact: true}">
          <a routerLink="/">Home</a>
        </li>
        <li role="presentation"
            routerLinkActive="active">
          <a routerLink="servers">Servers</a>
        </li>
        <li role="presentation"
            routerLinkActive="active">
          <a [routerLink]="['users']">Users</a>
        </li>
        
```

* routerLink='servers' navigates to relative path servers. If I am at ../servers link it will try to navigate to '.../servers/servers'
* routerLink='/servers' means thhe global path
* routerLink = '../servers' the same as in the folder structure


**Styling active tabs**

You can add routerLinkActive to the link or to the wrapping element
For home link we add  [routerLinkActiveOptions]="{exact: true}". Otherwise the link matches for all the other paths and the 'home' item would be highlighted


**Navigating programmatically**

```javascript
constructor(private router: Router) { }
...

    this.router.navigate(['/servers']);
```

Realatie routing:

```javascript
constructor(private serversService: ServersService,
              private router: Router,
              private route: ActivatedRoute) {
  }
  
...

this.router.navigate(['servers'], {relativeTo: this.route});
```

**Passing parameters**

Declaring routes with params:

```javascript
const appRoutes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'users/:id/:name', component: UsersComponent}
];
```

Fetching params values from the url:

```javascript

 ngOnInit() {
    this.user = {
      id: this.route.snapshot.params['id'],
      name: this.route.snapshot.params['name']
    };
    this.paramsSubscription = this.route.params
      .subscribe(
        (params: Params) => {
          this.user.id = params['id'];
          this.user.name = params['name'];
        }
      );
  }
  
 ```
 
 * If we use the snapshot method if we have a link to a url linked to the same controller the ngOnInit is not called anymore and therefore the page data won't be changed
 * In order to avoid this unwanted behaviour we use the observables. To avoid problems it is bette to unsubscribe, even though the routes observers are unsubscribed by default  by angular
 
 ```javascript
   ngOnDestroy() {
    this.paramsSubscription.unsubscribe();
  }
```

Query params in the url:

HTML:

```HTML

  <a
        [routerLink]="['/servers', server.id]"
        [queryParams]="{allowEdit: server.id === 3 ? '1' : '0'}"
        fragment="loading"
        href="#"
        class="list-group-item"
        *ngFor="let server of servers">
        {{ server.name }}
      </a>
      
```
Programmatically:

```javascript
this.router.navigate(['/servers', id, 'edit'], {queryParams: {allowEdit: '1'}, fragment: 'loading'});
```


 
 
  
 
