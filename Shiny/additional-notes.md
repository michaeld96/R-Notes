# Server Functions
## `observe()`
* When we use `observe()`, we want it to react to reactive values.
* Use it for updates to textboxes/pulldowns
* No newly create data is returned.
* `observeEvent()` is used for specific events that trigger reactions.
  
## `render()`

* Displays output at the user interface.
* `renderPlot()` and `plotOutput()`
* `renderText()` and `textOutput()`
* Creates new features in the app.
### Workflow of Render

1. Data input via input widget in user interface.
2. Transferred to the server.
3. Calculations are performed in the server.
4. Calculations are used for `renderPlot()` or `renderText()`.
5. Objects are displayed by `plotOutput()` or `textOutput()` in the user interface.

## `reactive()`

* A function
* Requires users iteraction
* Reactives are stored as tpye closure objects.
* use the object with `()` in the server code.