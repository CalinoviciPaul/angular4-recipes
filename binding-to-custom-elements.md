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
