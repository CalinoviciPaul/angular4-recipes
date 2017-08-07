## View encapsulation

```
@Component({
  selector: 'app-server-element',
  templateUrl: './server-element.component.html',
  styleUrls: ['./server-element.component.css'],
  encapsulation: ViewEncapsulation.Emulated // None, Native
})
```
the css for this component will be applied globaly. For this component the attributes are not added

Native uses the shadow dom of the browser. But there may be browsers that don't support shadow dom
