# Manim-web in Myst

This repository provides an example (and a guide) of using `manim-web` library
for creating animations with `myst`.

## Guide

0. Ensure you have `mystmd` installed, especially the version that support `widjets` plugin
1. Create myst project
2. In order to insert _html_ element with _js_ code, we use `widjets` myst
   plugin
   - By following the documentation, we create a `.mjs` file and use it in the
     document as a directive 
    ````js
    ```{anywidget} ./first-widget.mjs
    ```
    ````

3. Then we import `manim-web` library in the `.mjs` file with
    ```js
    import {Axes, Create, WHITE} from 'https://cdn.jsdelivr.net/npm/manim-web@0.3.22/dist/manim-web.browser.js';
    ```

4. Then, we create a render function and export it, this is required by `widgets` plugin
    ```js
    function render({ model, el }) {
    }
    export default { render };
    ```

5. In that function, we create a `div` and append it to `el`, then we create
   manim scene and call a function we will create just in a moment with
   `manim-web` code.
    ```js
    function render({ model, el }) {
      // Build your UI and append it to `el`
      let container = document.createElement('div');
      container.setAttribute("style","width:800px;height:450px;background:white;");
      container.setAttribute("id", "container");
      el.appendChild(container);

      //create scene
      let scene = new Scene(container, {
          width: 800,
          height: 450,
          backgroundColor: BLACK,
          backgroundOpacity: 1, // 0 = fully transparent, 1 = fully opaque
        });

        // call manim code
        manim_code(scene).then(() => {});
        // Standard normal PDF

    }
    ```

6. Write manim code in created `manim_code` **async** function
```js
async function manim_code(scene){
    // code here
  const axes = new Axes({
    xRange: [-4.5, 4.5, 1],
    yRange: [0, 0.45, 0.1],
    xLength: 9,
    yLength: 5,
    color: WHITE,
  });
  await scene.play(new Create(axes, { duration: 0.8 }));
}
```

7. Run `myst start` and watch your animation
