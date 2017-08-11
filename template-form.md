## Template driven approach form

- Add FormsModule in imports

HTML:
```HTML
----><form (ngSubmit)="onSubmit(f)" #f="ngForm">
        <div
          id="user-data"
          ngModelGroup="userData"
          #userData="ngModelGroup">
          <div class="form-group">
            <label for="username">Username</label>
            <input
              type="text"
              id="username"
              class="form-control"
 --->         ngModel
 --->         name="username"
              required>
          </div>
          <div class="form-group">
            <label for="email">Mail</label>
            <input
              type="email"
              id="email"
              class="form-control"
              ngModel
              name="email"
              required -->
              email --> checks if it is a valid mail
              #email="ngModel">
            <span class="help-block" *ngIf="!email.valid && email.touched">Please enter a valid email!</span>
          </div>
        </div>
        <p *ngIf="!userData.valid && userData.touched">User Data is invalid!</p>
        <div class="form-group">
          <label for="secret">Secret Questions</label>
          <select
            id="secret"
            class="form-control"
            [ngModel]="defaultQuestion"
            name="secret">
            <option value="pet">Your first Pet?</option>
            <option value="teacher">Your first teacher?</option>
          </select>
        </div>
        <div class="form-group">
          <textarea
            name="questionAnswer"
            rows="3"
            class="form-control"
            [(ngModel)]="answer"></textarea>
        </div>
        <p>Your reply: {{ answer }}</p>
        <div class="radio" *ngFor="let gender of genders">
          <label>
            <input
              type="radio"
              name="gender"
              ngModel
              [value]="gender"
              required>
            {{ gender }}
          </label>
        </div>
        <button
          class="btn btn-primary"
          type="submit"
          [disabled]="!f.valid">Submit</button>
      </form>  
```
.................................................................................................

```javascript
// onSubmit(form: NgForm) {
  //   console.log(form);
  // }
```
We can access the form with @ViewChild('f') signupForm: NgForm;

disable submit button if form is not valid [disabled]="!f.valid"

```HTML
...
 #email="ngModel">
<span class="help-block" *ngIf="!email.valid && email.touched">Please enter a valid email!</span>
```

**Grouping Form controls**

 ngModelGroup="userData"
 #userData="ngModelGroup"
 the same as for the email
 
 
 **Handling radio buttons**
 
 ```javascript
 
  <div class="radio" *ngFor="let gender of genders">
          <label>
            <input
              type="radio"
              name="gender"
              ngModel
              [value]="gender"
              required>
            {{ gender }}
          </label>
        </div>
        
```
 **Overriding and patching values in the form**
 
 ```javscript
 suggestUserName() {
    const suggestedName = 'Superuser';
    // this.signupForm.setValue({
    //   userData: {
    //     username: suggestedName,
    //     email: ''
    //   },
    //   secret: 'pet',
    //   questionAnswer: '',
    //   gender: 'male'
    // });
    this.signupForm.form.patchValue({
      userData: {
        username: suggestedName
      }
    });
  }
 ```
 

