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

- above command will create a _posts_ folder - then _post-create_ component with in the _posts_ folder.

  final output will be like this:

![image](./screenshots/screen-3.jpg 'image')

- here v create _post-create_ component in posts folder.

---
