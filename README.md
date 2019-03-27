# Shiny Dashboard Experiments    
Experimenting with ShinyApp to build interactive visualizations.

<img src="https://github.com/ajh1143/ShinyApp-Experiments/blob/master/ShinyApp_1.png" class="inline"/><br>


## Libraries
```R
library(shinydashboard)
library(ggplot2)
data(mtcars)
```

# Header
```R
header <- dashboardHeader( 
                          dropdownMenu(type = "notifications", 
                                       messageItem(
                                                  from = "Andrew",
                                                  message = "Check out Reddit for your favorite cat videos",
                                                  href = "https://www.reddit.com/r/doggos" 
                                                  
                                                  ) 
                                      ), 
                          
                          dropdownMenu(type = "messages",   
                                          messageItem(
                                                      from = "Andrew",
                                                      message = "Haha, got you. Here's some cats. ",
                                                      href = "https://www.reddit.com/r/cats"
                                                     )
                                      ),
                          dropdownMenu(type = "tasks", badgeStatus = "success",
                                       taskItem(value = 100, color = "green",
                                                "Documentation"
                                       ),
                                       taskItem(value = 50, color = "yellow",
                                                "Server deployment"
                                       ),
                                       taskItem(value = 10, color = "red",
                                                "Overall project"
                                       )
                          )
                          )
```
## Notifications
<img src="https://github.com/ajh1143/ShinyApp-Experiments/blob/master/ShinyApp_2.png" class="inline"/><br>
```R
dropdownMenu(type = "notifications", 
             messageItem(
                        from = "Andrew",
                        message = "Check out Reddit for your favorite cat videos",
                        href = "https://www.reddit.com/r/doggos" 

                        ) 
            )
```
## Messages
<img src="https://github.com/ajh1143/ShinyApp-Experiments/blob/master/ShinyApp_3.png" class="inline"/><br>
```R
 dropdownMenu(type = "messages",   
              messageItem(
                          from = "Andrew",
                          message = "Haha, got you. Here's some cats. ",
                          href = "https://www.reddit.com/r/cats"
                         )


```
## Tasks
<img src="https://github.com/ajh1143/ShinyApp-Experiments/blob/master/ShinyApp_4.png" class="inline"/><br>
```R
dropdownMenu(type = "tasks", badgeStatus = "success",
             taskItem(value = 100, color = "green",
                      "Documentation"
             ),
             taskItem(value = 50, color = "yellow",
                      "Server deployment"
             ),
             taskItem(value = 10, color = "red",
                      "Overall project"
             )
```

## Sidebar
<img src="https://github.com/ajh1143/ShinyApp-Experiments/blob/master/ShinyApp_Widgets.png" class="inline"/><br>
```R
sidebar <- dashboardSidebar(
                sidebarMenu(
                            menuItem("Dashboard", tabName = "dashboard", icon = icon("dashboard")),
                            menuItem("Widgets", tabName = "widgets", icon = icon("th"))
                            ), 
                selectInput( inputId = "DropDown",
                             label = "Drop Down Box",
                             choices = 1:3)
                )
```

### Body
```R
body <- dashboardBody( tabItems(
            # First tab content
            tabItem(tabName = "dashboard",
                    fluidRow(column(10, 
                                    box(plotOutput("plot1", height = 550), status = "primary", solidHeader = TRUE),

                                    box(title = "Controls", background='purple',status = "warning", solidHeader = FALSE,
                                    sliderInput("slider", "Number of observations:", 1, 255, 1),

                                    inputPanel(
                                    selectInput('x', 'x', choices = colnames(mtcars),
                                               selected='wt'),
                                    selectInput('y', 'y', choices = colnames(mtcars), 
                                               selected = 'mpg')
                                    ),
                                    checkboxInput('jitter', 'Jitter'),
                                    checkboxInput('smooth', 'Smooth'),
                                    checkboxInput('harsh', 'Harsh') 
                                  )
                            )  )
            ),

            # Second tab content
            tabItem(tabName = "widgets",
                    h2("Widgets tab content"),
                    fluidRow(tabBox( title = "Sample Box",
                                     tabPanel("Tab: 1","Content for the first tab"),
                                     tabPanel("Tab: 2","Content for the second tab")
                                    )
                             )
                    )

            )


 )
```
## Controls
<img src="https://github.com/ajh1143/ShinyApp-Experiments/blob/master/ShinyApp_Control.png" class="inline"/><br>
```R
# First tab content
tabItem(tabName = "dashboard",
        fluidRow(column(10, 
                        box(plotOutput("plot1", height = 550), status = "primary", solidHeader = TRUE),
                        box(title = "Controls", background='purple',status = "warning", solidHeader = FALSE,
                        sliderInput("slider", "Number of observations:", 1, 255, 1),
                        inputPanel(
                                   selectInput('x', 'x', choices = colnames(mtcars),
                                                                     selected='wt'),
                                   selectInput('y', 'y', choices = colnames(mtcars), 
                                                                  selected = 'mpg')
                                  ),
                                  checkboxInput('jitter', 'Jitter'),
                                  checkboxInput('smooth', 'Smooth'),
                                  checkboxInput('harsh', 'Harsh') 
                                  )
           
```
## UI
```R
ui <- dashboardPage(header, sidebar, body)
```

## Server
```R
server <- function(input, output){

                  output$plot1 <- renderPlot({
                              data_style <- input$slider
                              X <- input$x
                              Y <- input$y

                              # Create scatterplot

                             ggplot(data = mtcars, aes_string(x = X, y = Y))+
                                    geom_point(shape= data_style, size=10)+ 
                                    labs(title="MTCARS DATA", subtitle=paste0("Data Point Symbol Type : ", data_style))
                            })

                          # plot(data=mtcars, X, Y, main=paste0("Data Point Symbol Type : ", data_style), 
                              # xlab=X, ylab=Y, pch=data_style, xlim=c(0,10), ylim=c(0,50))})

                }
```
## GGPlot2 Scatterplot
<img src="https://github.com/ajh1143/ShinyApp-Experiments/blob/master/ShinyApp_Graph.png" class="inline"/><br>
```R
X <- input$x
Y <- input$y

# Create scatterplot

ggplot(data = mtcars, aes_string(x = X, y = Y))+
      geom_point(shape= data_style, size=10)+ 
      labs(title="MTCARS DATA", subtitle=paste0("Data Point Symbol Type : ", data_style))
```

## ShinyApp
```R
shinyApp(ui, server)
```
