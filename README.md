# Notes

- App Component is registered with angular.
  we can now see this App Component in other component.

![image](./screenshots/screen-1.jpg 'image')
**app.module.ts**

```typescript
  declarations: [
    AppComponent
  ],
```

- code in main.ts file is executed at first.

**main.ts**

```typescript
platformBrowserDynamic()
  .bootstrapModule(AppModule)
  .catch((err) => console.error(err));
```

---

## understanding angular component

![image](./screenshots/screen-2.jpg 'image')

- we can have different components in the app
  eg: app-header, app-brand, app-feature ..
  can have another components in app-header like app-nav-item, app-nav-search.

- in this app, we have to create, edit, delete post.

## adding new component

```bash
ng g c posts/post-create
```

- above command will create a _posts_ folder - then add _post-create_ component with in the _posts_ folder.

  final output will be like this:

![image](./screenshots/screen-3.jpg 'image')

- here v create _post-create_ component in posts folder.

- add contents in post-create template file

- add the post-create selector in app component.html

```html
<h1>Our First App</h1>
<app-post-create></app-post-create>
```

- thus the content in post-create component will be rendered in app component template.

---

## Listening to events - event binding

> event binding means a feature that allows angular to listen to events made by users.
> here v add input and clicking on button angular listen to that click event and execute some function there.

**post-create-component.html**

```html
<button (click)="onAddPost()">Save post</button>
```

**post-create-component.ts**

```typescript
 onAddPost(){
    alert("post added");
  }
```

- binding click event on button. while click, the assigned method is fired.

---

## outputting contents - property binding

- outputting content user entered
- can use string interpolation or property binding
- in string interpolation, gives the value to be outputted in double curly braces.
- in property binding, binds the value to be outputted to the value property of html element.
  Eg:

```html
<textarea name="post" id="post" cols="30" rows="5" [value]="newPost"></textarea>
<p>{{ newPost }}</p>
```

---

## getting user input

- accessing the element - getting the value.

- first set a reference to the element.

```html
<textarea
  name="post"
  id="post"
  cols="30"
  rows="5"
  [value]="newPost"
  #postInput
></textarea
><br /><br />
```

_postInput_ is the reference to the textArea element.

- other elements can access this element using postInput reference.

```html
<button (click)="onAddPost(postInput)">Add Post</button>
```

- pass the reference as an argument to _onAddPost_ method.

```typescript
onAddPost(postInput: HTMLInputElement){
    this.newPost = postInput.value;
  }
```

- _onAddPost_ method receive the element sent as _argument_.
- access the _value_ property of the same.

---

## getting and outputting data at same time - two way binding

- use ngModel directive - assign a property.

```html
<textarea
  name="post"
  id="post"
  cols="30"
  rows="5"
  [(ngModel)]="postValue"
></textarea
><br /><br />
```

> here v get the value to _textArea_ element and output the value from _textArea_ at same time. earlier v do property binding to output the value. and do a local reference to get the value. by using ngModel directive, v get the value from user and output the value on template at same time.

```typescript
  postValue: string ="";
  ngOnInit(): void {
  }
  onAddPost(){
    this.newPost = this.postValue;
  }
```

- declare _postValue_ property here, assign it to _newPost_.

- finally add **FormsModule** to the _imports_ array in _app module_.

---

## installing angular material

### create a form using angular material

(Angular Material)[https://material.angular.io/guide/getting-started]

(Angular Component)[https://material.angular.io/components/categories]

```html
<div class="container">
  <mat-card>
    <h3>Add Posts</h3>
    <mat-form-field class="example-form-field">
      <textarea
        rows="5"
        matInput
        [(ngModel)]="postValue"
      ></textarea></mat-form-field
    ><br />
    <button mat-raised-button color="primary" (click)="onAddPost()">
      Add Post
    </button>
    <br />

    <p>{{ newPost }}</p>
  </mat-card>
</div>
```

**app.module.ts**

```typescript
import { MatInputModule } from '@angular/material/input';
import { MatButtonModule } from '@angular/material/button';
import { MatCardModule } from '@angular/material/card';

imports: [MatInputModule, MatButtonModule, MatCardModule];
```

---
