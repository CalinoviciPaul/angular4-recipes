##Pipes

Pipe declaration:

@NgModule({
  declarations: [
    AppComponent,
    ShortenPipe,
    FilterPipe
  ],
...

Pipe implementation:


```javascript
@Pipe({
  name: 'shorten'
})
export class ShortenPipe implements PipeTransform {
  transform(value: any, limit: number) {
    if (value.length > limit) {
      return value.substr(0, limit) + ' ...';
    }
    return value;
  }
}
```

```HTML
 <strong>{{ server.name | shorten:15 }}</strong> 
 
 ```
 

