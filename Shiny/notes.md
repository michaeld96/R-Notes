# Shiny Notes

When we are working with Shiny there are important things to keep in mind. It is a reactive programming language. There are three fundamental building blocks

1. reactive values
2. reactive expressions
3. observers

We will learn more about this, but let's focus on basics of Shiny. In Shiny we have a UI and a Server function. The UI is designed in mind that UI will only run once. Server is designed to run multiple times. With each input in Shiny, there is a rendering output factor. Let's look at this simple example:

```R
library(shiny)

ui <- fluidPage(
  textInput("name", "What is your name?", placeholder = "Michael :)"),
  textOutput("greetings")
)

server <- function(input, output, session)
{
  output$greetings <- renderText({
    paste0("Hi there, ", input$name)
  })
}

shinyApp(ui = ui, server = server)
```

`textInput()` takes a input from the user which is named `name`. With every input in Shiny there is a reactive output that is usually paired together. `textOutput()` is a where the output is designed to be placed in the UI. `renderText()` is the pairing for `textInput()`. The next example we will show how to mess around with reactive values.

## Example 1: Slider Inputs and Reactive Outputs

Let's say we have two slider inputs and with this input we want to perform some manipulation on the number that is chosen. We need to think of how to update this number when the user wants to adjust this slider. This is where the idea of reactive values come into play. Let's see the code!

```R
library(shiny)

ui <- fluidPage(
  sliderInput(inputId = "x", label = "If x is", min = 1,  max = 50, value = 30),
  sliderInput(inputId = "y", label = "and y is", min = 1, max = 50, value = 2),
  textOutput("times"),
  textOutput("plus_five"),
  textOutput("plus_ten")
)

server <- function(input, output, session)
{
  product <- reactive(input$x * input$y)
  product_plus_five <- reactive(product() + 5)
  product_plus_ten <- reactive(product() + 10)
  
  output$times <- renderText({
    paste0("x times y is ", product())
  })
  output$plus_five <- renderText({
    paste0("x * y + 5 is ", product_plus_five())
  })
  output$plus_ten <- renderText({
    paste0("x times y + 10 is ", product_plus_ten())
  })
}

shinyApp(ui = ui, server = server)
```

We see that `product` is equal to some expressions that are wrapped in `reactive()`, this makes it so that every time the inputs are updated then the computation is repeated. The reason we see `product()` is that we are getting the numeric value from this; it is naturally a function.  

## Example 2: Plotting with Shiny

Below is some code that is using reactivity and plotting using Shiny. Remember, when dealing with anything that is going to change constantly from the user needs to be wrapped in `reactive()`.

```R
library(shiny)
library(ggplot2)

datasets <- c("economics", "faithfuld", "seals")

ui <- fluidPage(
  selectInput(inputId = "dataset", "Dataset", choices = datasets),
  verbatimTextOutput("summary"),
  plotOutput("plot")
)

server <- function(input, output, session)
{
  dataset <- reactive({
    get(input$dataset, "package:ggplot2")
  })
  output$summary <- renderPrint({
    summary(dataset())
  })
  output$plot <- renderPlot({
    plot(dataset())
  }, res = 94)
}

shinyApp(ui, server)
```

Here, let's note that `selectInput()` is a function in base Shiny that creates a list for the user to choose from. Also, `plot()` is by default a scatter plot.

## Basic UI

There is common structure when it comes to input names. The first argument is always `inputId` which will be in the `input` list when being used in the server. Basically, this identifier will be used to connect the front end with the back end. Most function's second parameter is called `label` which will be displayed on the UI where the input is on the screen. The third is `value` which lets you set the default value.

### Free Text Areas

 Shiny can collect small amounts of text using `textInput()`, passwords with `passwordInput()`, and paragraphs with `textAreaInput()`. If you want make sure the user is using certain properties, `validate()`, which validates that an output has all the necessary inputs.

### Numeric Inputs

 For our apps to collect numeric inputs we can use `sliderInput()` to have a bar that slides from a min and max value, and the user selects the value they want. There is also a input that a user can type in and set up or down by $1$. This function is `numericInput()`. Sliders are highly customizable!

## TO BE CONTINUTED ABOVE

## Shiny Router  

With the growth of our Shiny apps we build more and more files, and when we launch our Shiny app to the web, the URL will always be the same. This can be an ugly situation. Let's say that the user wants to go back to the previous page, they cannot based on the `/` path that the URL is based on. To add paths, we will use `shiny.router`.

Let's look at a basic `shiny.router` example:

## Example ? Layout of Shiny Router

```R
library(shiny)
library(shiny.router)

home_page <- div(h2("Home Page"))
other_page <- div(h3("Other page"))

router <- make_router(
  route("/", home_page),
  route("other", other_page)
)

ui <- fluidPage(
  title = "Router demo",
  router$ui
)

server <- function(input, output, session) {
  router$server(input, output, session)
}

shinyApp(ui, server)
```

We make two variables named `home_page` and `other_page`; these will serve as basic HTML pages. Next, we have `make_router()`. `make_router()` makes the routes to the pages based on what is put in the parameters. We place what is called `routes()` inside of `make_router()`. `routes()` take in a `path`, `ui`, and `server`. When this path s called it brings the pages defined `ui` and `server`. Let's Look at a more complicated example with some CSS:

## Example ? Router Example with Server

```R
library(shiny)
library(shiny.router)

web_page <- function(title_of_page, content)
{
  div(
    class = "web-page",
    h2(title_of_page),
    tags$p(content),
    uiOutput("page_button"),
    textOutput("text_goes_here")
  )
}

other_page <- function(title_of_other, content_other)
{
  div(
    class = "web-page",
    h2(title_of_other),
    tags$p(content_other),
    textOutput("other_text"),
    uiOutput("home_button")
  
  )
}

home_page_ui <- web_page("Home Page", "here is some content")
other_page_ui <- other_page("Other Page", "Other page's content, wow :)")

# You can assign each router with it's own function.

home_page_server <- function(input, output, session)
{
  output$text_goes_here <- renderText({
    paste("Here is some render text.")
  })
  output$page_button <- renderUI({
    actionButton(inputId = "other_page_button", label = "Go to Other Page")
  })
}

other_page_server <- function(input, output, session)
{
  output$other_text <- renderText({
    "Here is some rendered text."
  })
  output$home_button <- renderUI({
    actionButton(inputId = "home_page_button", label = "Go Home!")
  })
}

# This is where we define the paths, what function is the pages UI and server.
router <- make_router(
  route(path = "/", ui = home_page_ui, server = home_page_server),
  route(path = "other", ui = other_page_ui, server = other_page_server)
)

ui <- fluidPage(
  tags$head(
    tags$link(rel = "stylesheet", href = "styles.css")
  ),
  router$ui
)

server <- function(input, output, session) {
  # Call the server functions that are defined in make_router.
  router$server(input, output, session)
  observeEvent(input$other_page_button, {
    if(is_page("/"))
    {
      change_page("other")
    }
  })
  observeEvent(input$home_page_button, {
    if(is_page("other"))
    {
      change_page("/")
    }
  })
}

shinyApp(ui, server)
```

```css
/******* Start of CSS *******/
body
{
  background-color: #e9e9e9;
  padding: 0px;
  margin: 0px;
}
.web-page
{
  padding: 10px;
  width: 50%;
  text-align: center;
  height: 10%;
  margin: 0 25%;
  background-color: #ADD8E6;
  margin-top: 15%;
  margin-left: 23%;
  border-radius: 15px;
  box-shadow: 7px 6px 9px rgb(0 0 0 / 10%);
}
/******* End of CSS *******/
```

Looking at the code, let's break some things down. We see that `home` and `other_page` have a `ui` and `server`. This is then to be called in the `route` function inside of `make_router()` It's important to know that `/` is the home directory, as in, this is where the site will initially launch to. When calling the server function inside of `server` we need to have `router$server(input, output, server)`. `router` can be renamed, but this is the name we chose to give to our `make_router()`. Also, notice that we have an `observeEvent()`. This is here to give a response to our `actionButton` for switching pages. When we click our `actionButton` we trigger this response; if we are on the home page, `/`, then we will `change_page` to `"other"`. Looking at `is_page()`, this checks to see if the user is on  `path == /`, if they are it changes to the other page by using the function `change_page()`. Notice that the `path` names are the same the `page` names.

## Shiny Router with Modules

Now that we have an idea of `shiny.router`, it's time to make use of the idea of namespace. Namespace is a very important tactic that can be used when we are building functions that are used over and over again. With reactive programming, if there isn't a different namespace, there is conflicting issues that happen and the reaction of the app is effected, sometimes not working at all.

Below is an example, it takes from the example above, but this makes use of modules.

## Example ? Shiny Router with Modules

```R
library(shiny)
library(shiny.router)

web_page <- function(title_of_page, content, id)
{
  #All inputs and outputs must be wrapped with ns() inside of UI.
  ns <- NS(id)
  div(
    class = "web-page",
    h2(title_of_page),
    tags$p(content),
    uiOutput(ns("page_button")),
    textOutput(ns("text_goes_here"))
  )
}

main_server <- function(input, output, session)
{
  ns <- session$ns
    output$text_goes_here <- renderText({
      paste("Here is some render text.")
    })
    output$page_button <- renderUI({
      actionButton(inputId = ns("other_page_button"), label = "Go to Other Page")
    })
    observeEvent(input$other_page_button, {
      if(is_page("/"))
      {
        change_page("other")
      }
      if(is_page("other"))
      {
        change_page("/")
      }
    })
}

home_page_server <- function(input, output, session)
{
  moduleServer("HOME", main_server)
}

other_page_server <- function(input, output, session)
{
  moduleServer("OTHER", main_server)
}


home_page_ui <- web_page("Home Page", "here is some content", "HOME")
other_page_ui <- web_page("Other Page", "Other page's content, wow :)", "OTHER")

router <- make_router(
  route(path = "/", ui = home_page_ui, server = home_page_server),
  route(path = "other", ui = other_page_ui, server = other_page_server)
)

ui <- fluidPage(
  tags$head(
    tags$link(rel = "stylesheet", href = "styles.css")
  ),
  router$ui
)

server <- function(input, output, session) {
  # Call the server functions that are defined in make_router.
  router$server(input, output, session)
}

shinyApp(ui, server)
```

The very first function we write is the `web_page` that will serve as a general layout for each webpage we want to have. Notice that there is a third parameter called `id`. This `id` will serve as the namespace that will be passed into it, making each call to this function unique.

An important thing to remember when using modules is that all inputs and outputs need to be wrapped with `ns()`. An example from above is in `actionButton`. We see that it has a `inputId` of `"other_page_button"`, but since it is a output we need to wrap it with `ns()`.

## THIS MIGHT NOT BE THE CASE. EDIT IF NEED BE

Now, we need a main server that each server will call. Inside of this main server, we need to grab the namespace this being used in this instance. We do this by `ns <- session$ns`. We grab from the session list and find `ns` and store it into a local variable named `ns`. Inside this main server, we can perform anything we want that we would perform inside of a regular server.

Since we have that done, the next part is to connect each router to this main server. Each webpage will be using this server, but it needs its own namespace in order to communicate with it properly. This is why we see:

```R
home_page_server <- function(input, output, session)
{
  moduleServer("HOME", main_server)
}
```

The first parameter in `moduleServer` is the namespace you want to pass in. This namespace needs to match the namespace that we've assigned to the UI in order to get proper communication.

## Rhino Framework and Shiny Router