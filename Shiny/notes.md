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

### Dates

When using dates we can either choose one day with `dateInput()`, or we can choose a range of dates with `dateRangeInput()`. These will show a calendar and can be customizable with `format`, `language`, and `weekstart`.

## Example 3: Using Dates

```R
library(shiny)

ui <- fluidPage(
  dateInput(inputId = "dob", label = "What date were you born on?", format = "mm-dd-yyyy"),
  dateRangeInput("semester", "What are the dates for FALL 2022?", format = "mm-dd-yyyy")
)

server <- function(input, output, session)
{
  
}

shinyApp(ui, server)
```

### Limited Choices

When we want to limit the choices that a user can make we will use these two options: `selectInput()` and `radioButtons()`. If you want to select multiple things not using radio buttons you can use `checkboxGroupInput()`. See the example below:

## Example 4: Using Radio Buttons and Select Inputs

```R
library(shiny)

eecs_classes <- c("EECS 183", "EECS 203", "EECS 280", "EECS 281", "EECS 370", "EECS 376")
stem_subjects <- c("MATH", "CS", "DS", "STATS", "PHYSICS", "EE")

ui <- fluidPage(
  radioButtons("fav_eecs_class", "What is your favorite EECS class?", eecs_classes),
  selectInput("state", "What state are you from?", state.name, selected = "Michigan", multiple = TRUE),
  checkboxGroupInput("stem_classes", "What are some of your favorite STEM classes?", stem_subjects)
)

server <- function(input, output, session)
{
  
}

shinyApp(ui, server)
```

Note: When we use `selectInput()` on the UI side, if the input list is very large, it would make better use to use `selectInput()` on the server side because it would increase performance.  

### File Uploads

Letting the user upload files is important when working in data science. When uploading a file it requires some special handling on the server side, but later on in these notes we will focus on that.

## Example 5: File Upload Input

```R
library(shiny)

ui <- fluidPage(
  fileInput("file_input", "Upload File Here:")
)

server <- function(input, output, session)
{
  
}

shinyApp(ui, server)
```

### Action Buttons

Action buttons let a user perform something by just hitting a button. Here are two, `actionButton()`, and `actionLink()`. These two are usually paired with `observeEvent()` or `eventReactive()` in the server function. Later on we will learn how to implement these functions inside of our server. Also, the appearance of these buttons can be customized with the `class` argument by using `"btn-primary"`, `"btn-success"`, `"btn-info"`, `"btn-warning"`, or `"btn-danger"`. The button can also be changed with `"btn-lg"`, `"btn-sm"`, or `"btn-xs"`. Last thing we'll mention on action buttons is that they can also span the whole width of the page using `"btn-block`. Here are some button examples:

## Example 6: Action Buttons

```R
library(shiny)

ui <- fluidPage(
  actionButton("calendar", "Go to Calendar", class = "btn-info"),
  actionButton("drinks", "Bars near Me", icon = icon("cocktail"), class = "btn-primary")
)

server <- function(input, output, session)
{
  
}

shinyApp(ui, server)
```

## Example 7: Having Slider Input, but with Dates

```R
library(shiny)

ui <- fluidPage(
  sliderInput(inputId = "dates", "When should we deliver?", min = as.Date("2020-09-16"), 
              max = as.Date("2020-09-23"), value = as.Date("2020-09-17"), timeFormat = "%F")
)

server <- function(input, output, session)
{
  
}

shinyApp(ui, server)
```

## Example 8: SliderInput Animation Loop

With this example we look at `sliderInput()` and how `animation` works on it. We made a slider with a `step = 5` and the animation will play 1000 times. Also, `loop = TRUE`, so when the animation reaches the end the max the animation will start from the beginning.

```R
library(shiny)

ui <- fluidPage(
  sliderInput("animate", "What me Go!", min = 0, max = 100, value = 0, step = 5, 
              animate = animationOptions(interval = 1000, loop= TRUE))
)

server <- function(input, output, session)
{
  
}

shinyApp(ui, server)
```

## Example 9: Select Input with Categories

This example we look into a long list, but we want to group this list by traits they share. With this example we use classes that a student has taken, and we look at these classes by subject.

```R
library(shiny)

ui <- fluidPage(
  selectInput("classes_taken", "Classes I've Taken By Catagory", 
              list(`EECS` = list("EECS 183", "EECS 280", "EECS 281"),
                   `MATH` = list("MATH 115", "MATH 116", "MATH 214", "MATH 215"),
                   `PHYSICS` = list("PHYSICS 140", "PHYSICS 141", "PHYSICS 240", "PHYSICS 241"))
              )
)

server <- function(input, output, session)
{
  
}

shinyApp(ui, server)
```

## Basic Outputs

Outputs in UI are just placeholders for rendering functions from the server. This placeholder tells the UI where it is wanting to place the output. Outputs follow the same idea of inputs where the first parameter is the `"id"`. So say we use `textOutput()`, well the first parameter of this function is the `outputId` which is then used to communicate with the server to see what is going to be outputted. Note, each output is paired with a `render` function in the server. In base Shiny there are three types of outputs: text, tables, and plots.

### Text

 There are two types of outputs we want to use when it comes to text: `textOutput()` for regular text, and `verbatimTextOutput()` for code and console output.

## Example 10: Using `textOutput()` and `verbatimTextOutput()`

 This example will look at how outputs pair with a rendering function in the server. Here we are just seeing how these placeholders work in the UI and how they render inside the server function.

 ```R
 library(shiny)

ui <- fluidPage(
  textOutput("text_goes_here"),
  verbatimTextOutput("code_text_here")
)

server <- function(input, output, session)
{
  output$text_goes_here <- renderText({
    "Hi there!"
  })
  # summary is a in house R function.
  output$code_text_here <- renderText({
    summary(1:100)
  })
}

shinyApp(ui, server)
 ```

 Note, if we wanted to print out what we would see in the RStudio console, then we would use `verbatimTextOutput()` paired with `renderPrint()`. This idea is illustrated in the next example.

## Example 11: Printing RStudio Console to the UI

 ```R
 library(shiny)

vec <- c(1, 2, 3, 4, 5)

ui <- fluidPage(
  verbatimTextOutput("console_output")
)

server <- function(input, output, session)
{
  output$console_output <- renderPrint({
    print("Howdy")
    #Prints a vector to the output just like it 
    #would in console.
    print(vec)
  })
}

shinyApp(ui, server)
 ```

### Tables

In Shiny there are two ways to display data frames in tables:

* `tableOutput()` paired with `renderTable()`. This renders a static table of data that will show all the data at once.

* `dataTableOutput()` paired with `renderDataTable()`. This will render a dynamic table, showing a fixed number of rows along with controls to change which rows are visible.

When using these tables, its good to know when to use them. When we want to use `tableOutput()`, we want to use it for small fixed summaries. Using `dataTableOutput()` is good for showing a complete data frame. _Note:_ A good library for having complete control on the output is `reactable`.

```R
library(shiny)

ui <- fluidPage(
  tableOutput("static"),
  dataTableOutput("dynamic")
)

server <- function(input, output, session)
{
  output$static <- renderTable(head(mtcars))
  output$dynamic <- renderDataTable(mtcars, options = list(pageLength = 5))
}

shinyApp(ui, server)
```

_Note:_ We do not need `{}` for inside of  `renderTable()` or `renderDataTable()`

### Plots

With Shiny's outputs we can display any type of R graphic with `plotOutput()` paired with `renderPlot()`. R graphic meaning any base plot, ggplot2, etc.

## Example 12: Rendering Basic Plot


In this example we are going to make a quadratic graph using `curve()`. We first make a simple R function that is going to return $x^2$ to the when this function is called. Next, we write a placeholder in the UI section; this is where the plot will be displayed. After this, we will write the `renderPlot()` and we will use `curve()` here to give us a nice graph displaying $f(x) = x^2$. `res = 98` is telling the graph that we want this PNG to render with this resolution.

```R
library(shiny)
library(shinydashboard)
quad <- function(x)
{
  return(x^2)
}

ui <- fluidPage(
  plotOutput("plot", width = "400px", height = "400px")
)

server <- function(input, output, session)
{
  output$plot <- renderPlot(curve(exp = quad, from = -10, to = 10), res = 96)
}

shinyApp(ui, server)
```

_Note:_ It's recommended that we keep the `res = 96` because this will make the plot look as close as it can when it is rendered in RStudio.

### Downloading

We can let users download a file with `downloadButton()` or `downloadLink()`, but this will require some new techniques in the server function that we will get to later in these notes.

## Basic Reactivity

When it comes to reactive programming, it can be quite different than scripts. The basic idea is we have decencies on some input, and when that input changes than the output also changes. We do this by  using reactive values. When these reactive values change the output is updated with these changed values.

### Input

The `input` parameter is a list-like object that contains all of the inputs sent from the browser by the `inputId`. Inputs from the browser, `inputId`, are read-only. If modifications are tried on them they will throw an error.

Input's are selective on who it is read by. For `inputId` to be read, it must be in a reactive context created by a function like `renderPrint()` or `reactive()`.

### Output

`output` is almost like `input` by being a list-like object. The main difference between the two is that we are sending out output rather than receiving input. We will always use the `output` object in concert with a `render` function.

With render functions it does two things:

1. It sets up a special reactive context that automatically tracks what inputs the output uses.
2. It converts the output of our R code into HTML that is suitable for the webpage.

### Reactive Programming

We will start our journey with reactive programming by first looking at this example:

## Example 13: Constant Rendering

```R
library(shiny)

ui <- fluidPage(
  textInput("name", "What is your name?"),
  textOutput("response")
)

server <- function(input, output, session)
{
  output$response <- renderText({
    paste0("Hello ", input$name, "!")
  })
}

shinyApp(ui = ui, server = server)
```

Running the code above we see that every time we type in a letter the server will render it. We can see that this is inefficient. Rather than doing that, let's store the input in a reactive value, and then use `renderText()` to display it in the browser:

## Example 14: Reactive Variable Rendering

```R
library(shiny)

ui <- fluidPage(
  textInput("name", "What is your name?"),
  textOutput("greeting")
)

server <- function(input, output, session)
{
  string <- reactive({
    paste0("Hello ", input$name, "!")
  })
  #Give us a latency when the user is typing name 
  output$greeting <- renderText(greetings())
}

shinyApp(ui = ui, server = server)
```

### Executive Order

The order of our Shiny application is only determined by the reactive graph: Look below at what this graph looks like:

![picture of reactive graph](../Media/reactive-graph.png)

