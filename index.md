## Control Charts:

#### What is a monitoring chart? 
-  These kinds of charts are used in process monitoring to find and detect changes over time. Control charts usually have 3 main components:
    -  Centerline: Could be mean of the data points that we are monitoring or could be a target value based on the problem specification. 
    -  Upper control limit: Horizontal line that shows the upper boundary of the in-control process.
    -  Lower control limit: Horizontal line that shows the upper boundary of the in-control process.
-  examples of monitoring charts, e.g. x-bar chart, range chart, EWMA, etc.
#### When we need control charts?
- When controlling ongoing process by finding anomalies and correcting the problems.
- When determining whether a process is stable or not.
- When predicting the outcomes of a process.
#### Why we need control charts?
- To estimate process average and variation. 
- To judge the impact of process improvement methods.
- To prevent out of control process by finding the correlation of different variables and the effect of the on each other. 


## Implementing control charts:
- ### Two Phases:
    - #### Phase I:
        Finding most reliable control limits based on the in-control process data points. In other words, finding in-control process data points   
        - There different methods based on the data type. We apply Moving Range method because the sample consists of an individual unit (working with real time data)
```markdown
FilteringOutliers <- function(dataset, colNumber, initialTrainset, L, lambda, maxp, maxq, maxd) {
  temp <- initialTrainset _# number of data points to train the model_
  datasets <- UniqueVariableLists(dataset, colNumber) _# if we have different variables in data set we seperet data based on those variables_
  phaseIparameters <- list()
  phaseIewmaParameters <- list()
  for(i in datasets){
    initialData <- InitialTrainingData(inputdata = i, initialTrainset)
    parameters <- TrainModel(initialData, L, lambda, maxp, maxq, maxd)
    ewmaParameters <- InitEwmaParameters()
    ewmaParameters <- InitialEWMAPhaseI(ewmaParameters, parameters)
    while(TRUE %in% (ewmaParameters$ewma_all > ewmaParameters$controlLimits$ucl_n1 | 
                     ewmaParameters$ewma_all < ewmaParameters$controlLimits$lcl_n1)) {  
      removedDatapoints <- length(parameters$arimaModel$x[(ewmaParameters$ewma_all > ewmaParameters$controlLimits$ucl_n1 |
                                                             ewmaParameters$ewma_all < ewmaParameters$controlLimits$lcl_n1)])
      parameters$arimaModel$x <- parameters$arimaModel$x[! parameters$arimaModel$x %in% 
                                                           parameters$arimaModel$x[(ewmaParameters$ewma_all > 
                                                                                      ewmaParameters$controlLimits$ucl_n1 | 
                                                                                      ewmaParameters$ewma_all < 
                                                                                      ewmaParameters$controlLimits$lcl_n1)]]
      parameters$arimaModel$x <- append(parameters$arimaModel$x, i[(temp + 1):(temp + removedDatapoints),2])
      temp <- temp + removedDatapoints
      parameters <- TrainModelPhaseI(parameters$arimaModel$x, parameters$pre_grade, L, lambda, maxp, maxq, maxd)
      ewmaParameters <- InitEwmaParameters()
      ewmaParameters <- InitialEWMAPhaseI(ewmaParameters, parameters)
    }
    phaseIparameters[[parameters$pre_grade]] <- parameters
    phaseIewmaParameters[[parameters$pre_grade]] <- ewmaParameters
  }
  `return(list(phaseIparameters = phaseIparameters))`
}
```

   - #### Phase II:
Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for


Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/VahabN/Control-charts/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
