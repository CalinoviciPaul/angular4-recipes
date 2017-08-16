## Reactive approach form

app-modules.ts:
```javascript
 imports: [
    BrowserModule,
    HttpModule,
    ReactiveFormsModule
  ]
```
Form initialization:

```javascript

 this.signupForm = new FormGroup({
      'userData': new FormGroup({
        'username': new FormControl(null, [Validators.required, this.forbiddenNames.bind(this)]),
        'email': new FormControl(null, [Validators.required, Validators.email], this.forbiddenEmails)
      }),
      'gender': new FormControl('male'),
      'hobbies': new FormArray([])
    });
```
Link the form defined in javascript with the html form:

```HTML
 ---> <form [formGroup]="signupForm" (ngSubmit)="onSubmit()">
        <div formGroupName="userData">
          <div class="form-group">
            <label for="username">Username</label>
            <input
              type="text"
              id="username"
-->              formControlName="username"
              class="form-control">
            <span
              *ngIf="!signupForm.get('userData.username').valid && signupForm.get('userData.username').touched"
              class="help-block">
              <span *ngIf="signupForm.get('userData.username').errors['nameIsForbidden']">This name is invalid!</span>
              <span *ngIf="signupForm.get('userData.username').errors['required']">This field is required!</span>

            </span>
          </div>
          <div class="form-group">
            <label for="email">email</label>
            <input
              type="text"
              id="email"
-->              formControlName="email"
              class="form-control">
            <span
              *ngIf="!signupForm.get('userData.email').valid && signupForm.get('userData.email').touched"
              class="help-block">Please enter a valid email!</span>
          </div>
        </div>
        <div class="radio" *ngFor="let gender of genders">
          <label>
            <input
              type="radio"
              formControlName="gender"
              [value]="gender">{{ gender }}
          </label>
        </div>
        <div formArrayName="hobbies">
          <h4>Your Hobbies</h4>
          <button
            class="btn btn-default"
            type="button"
            (click)="onAddHobby()">Add Hobby</button>
          <div
            class="form-group"
            *ngFor="let hobbyControl of signupForm.get('hobbies').controls; let i = index">
            <input type="text" class="form-control" [formControlName]="i">
          </div>
        </div>
        <span
          *ngIf="!signupForm.valid && signupForm.touched"
          class="help-block">Please enter valid data!</span>
        <button class="btn btn-primary" type="submit">Submit</button>
      </form>
 ```
 
 **Arrays of form control**
 
 ```HTML
  <div formArrayName="hobbies">
          <h4>Your Hobbies</h4>
          <button
            class="btn btn-default"
            type="button"
            (click)="onAddHobby()">Add Hobby</button>
          <div
            class="form-group"
            *ngFor="let hobbyControl of signupForm.get('hobbies').controls; let i = index">
            <input type="text" class="form-control" [formControlName]="i">
          </div>
        </div>       
 ```
........................................................................................................

```javascript
 onAddHobby() {
    const control = new FormControl(null, Validators.required);
    (<FormArray>this.signupForm.get('hobbies')).push(control);
  }
```

**Custom validators**

Declare custom validator:

```javascript
 'username': new FormControl(null, [Validators.required, this.forbiddenNames.bind(this)]),
 ```
 The validator is called by Angular so we have to bind the method to the current class
 
 Implementation
 ```javascript
  forbiddenNames(control: FormControl): {[s: string]: boolean} {
    if (this.forbiddenUsernames.indexOf(control.value) !== -1) {
      return {'nameIsForbidden': true};
    }
    return null;
  }
 ```
 
 **Error codes**
 
 ```javascript
  <span  *ngIf="!signupForm.get('userData.username').valid && signupForm.get('userData.username').touched"
              class="help-block">
              <span *ngIf="signupForm.get('userData.username').errors['nameIsForbidden']">This name is invalid!</span>
              <span *ngIf="signupForm.get('userData.username').errors['required']">This field is required!</span>

            </span>
 ```
 
 **Async validator**
 
 ```javascript
 
  forbiddenEmails(control: FormControl): Promise<any> | Observable<any> {
    const promise = new Promise<any>((resolve, reject) => {
      setTimeout(() => {
        if (control.value === 'test@test.com') {
          resolve({'emailIsForbidden': true});
        } else {
          resolve(null);
        }
      }, 1500);
    });
    return promise;
  }
}

```
Durign the validation process the css class of the component will be `ng-pending`
 
**Status and value listener**

```javascript
 // this.signupForm.valueChanges.subscribe(
    //   (value) => console.log(value)
    // );
    this.signupForm.statusChanges.subscribe(
      (status) => console.log(status)
    );
```

**Set and patch values**

```javascript
this.signupForm.setValue({
      'userData': {
        'username': 'Max',
        'email': 'max@test.com'
      },
      'gender': 'male',
      'hobbies': []
    });
    this.signupForm.patchValue({
      'userData': {
        'username': 'Anna',
      }
    });
 ```
