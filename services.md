## Services

**Injecting service**

```javascript
...
providers: [AccountsService, LoggingService]
...
```
Angular creates an instance of the service for a component and for all children components
If the service is specified in AppModule then the service is available application-wide (also for other services)

**Injecting services into other services **

```javascript
@Injectable()
export class AccountsService {
  accounts = [
    {
      name: 'Master Account',
      status: 'active'
    },
    {
      name: 'Testaccount',
      status: 'inactive'
    },
    {
      name: 'Hidden Account',
      status: 'unknown'
    }
  ];
  statusUpdated = new EventEmitter<string>();

  constructor(private loggingService: LoggingService) {}

  addAccount(name: string, status: string) {
    this.accounts.push({name: name, status: status});
    this.loggingService.logStatusChange(status);
  }

  updateStatus(id: number, status: string) {
    this.accounts[id].status = status;
    this.loggingService.logStatusChange(status);
  }
}

```

**Cross components comunication with services **

I create an event emitter in the service. In a component I emit the event and in another component I subscribe to the event

```javascript
    this.accountsService.statusUpdated.emit(status);
........................................................................................................

this.accountsService.statusUpdated.subscribe(
      (status: string) => alert('New Status: ' + status)
    );
    
```




