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


** Styling active tabs **

You can add routerLinkActive to the link or to the wrapping element
For home link we add  [routerLinkActiveOptions]="{exact: true}". Otherwise the link matches for all the other paths and the 'home' item would be highlighted


** Navigating programmatically **

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


