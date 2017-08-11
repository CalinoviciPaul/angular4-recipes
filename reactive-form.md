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
