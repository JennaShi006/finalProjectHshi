# finalProjectHshi

## Abstract
This R package is aims to streamline the data preparation process by offering various functions for cleaning, transforming, and analyzing data sets. The package consolidates multi-step workflows into modular functions, greatly increasing efficiency. Key features include tools for standardizing column names, handling missing values, scaling data, and dynamically generating new columns based on user-defined conditions. The package also provides utilities for statistical analysis, including functions to calculate group-wise means, generate regression equations, and compute probabilities from logistic regression models. By incorporating error handling and user-friendly parameters, the package is applicable to a slew of data sets and user needs.

## Introduction
While exploring ideas to focus my package on, I found myself continually returning to the long, multi-step data preparation and analysis explored in class and the homework. These analyses, due to their winded nature, often become confusing as the coder needs to sift through multiple functions and each of their usages. This introduces inefficiency and room for inaccuracy. I hope to help streamline this process by creating a package that consists of functions that each perform a specific combination of operations. 

These processes fall under the data preparation portion of the data science workflow. Data preparation involves data processing, profiling, cleansing, validation, and transformation. In the past, data cleaning and transformation relied heavily on manual coding with limited automation, faced challenges in integrating disparate sources, and lacked reproducibility. It was also a very specialized task since few people knew all the coding required to transform the data. Today, advancements include AI-assisted cleaning, automated feature engineering, and real-time extract, transformation, and load (ETL) pipelines. Tools that require minimal coding and domain-specific solutions have also made data preparation accessible to non-technical users.

My R package seeks to improve efficiency in specifically the data cleaning and transformation aspects by creating user friendly functions that simplify complex processes. 

## Design Capabilities and Philosophy
My R package offers various data cleaning, transformation, and analysis capabilities. For the purposes of data cleaning, the package includes functions like clean.names() and replace.NA.with(). Clean.names() allows users to format column names in a standardized manner by replacing certain characters with the desired characters in the char and newChar parameters. Replace.NA.with() allows users to gracefully handle NA values by replacing them with the value passed into the newVal parameter across multiple columns. The columns parameter also allows users to choose which columns in the data frame to apply the function to. 

The package also includes the functions conditional.col() and scale.vals(). The conditional.col() function allows the user to create a new column in a data frame and assign different values to each observation based on certain conditionals. The conditionals parameter asks for a list of all the conditions and their assigned values, keeping the interface simplified for the user by eliminating longwinded pipeline while maintaining flexibility. The scale.vals() function allows the user to scale the values of certain columns by a numeric value. This can be useful when the user would like to standardize values across multiple columns.

This package also contains four functions that aid in the analysis of data: mean.by.group(), lin.reg(), log.reg(), and calc.prob(). Mean.by.group() allows the user to first choose what variable they would like to group the data by, and then calculates the means of another variable for each grouping and outputs this analysis in a simple tibble. The functions lin.reg() and log.reg() produce the written format of the linear and logistic regression equations, respectively. The user can enter in a dataset, the response variable, and the predictors and expect a written-out format with the coefficients and their respective variables. The calc.prob() function is slightly different in that it requires the user to enter in a general linear model, and the function will calculate the probability of the response variable being 1 in a logistic regression model with certain predictors.

The function vis.palette() was motivated by the need for me to visualize the palette that I created myself within my R package. This function allows the user to enter any vector of colors and will produce a barplot with the colors. 

All functions besides calc.prob() and vis.palette() have data, a data frame, as the first argument. Functions like lin.reg() and log.reg() also share the same parameters so that the user can have a sense of structure and familiarity, and reduces the learning curve when it comes to using these functions. 


## Mechanics
In order to format the column names, the function clean.names() employs gsub() to replace specific characters (e.g., spaces) with user-defined replacements (e.g., periods), and tolower() to convert all column names to lowercase. The parameters char and newChar also have default values of “ ” and “.” respectively. 

Replace.NA.with() traverses through each column in the columns vector and assigns the user-defined value to any element that is NA using column indexing. Scale.vals() similarly traverses through the columns vector, and then multiplies all the values in each column by the parameter scale. Scale.vals() also has error handling to ensure that the scale parameter is a numeric value. For both of these functions, if the user does not enter a value for the columns parameter, the function will default to traversing through all columns. 

Conditional.col() first creates an empty new column with the name passed into colName and default values of NULL. Then, the function loops through each conditional in the conditionals vector, and uses rlang::f_lhs(cond) to extract the expression to be evaluated, and rlang::f_rhs(cond) to extract the value to be assigned according to the conditions. The function then assigns the new value to the observations for which that conditional is true. It also has error handling to ensure that the conditionals parameter is a list.

The mean.by.group() function depends on dpylr functions and pipelines, and thus has error handling to ensure that the dplyr package is installed and loaded by the user. The function then uses group_by() to group the data according to the group parameter. Then, within summarize(), the mean of the variable parameter is calculated for each of these groups.

Both lin.reg() and log.reg() functions are formatted similarly where a formula variable is created by combining the response and predictors, and pasting them with the “~” and “+” that are required by glm(). A model is then built (lm() for lin.reg() and glm() for log.reg). The coefficients are extracted from the model and then formatted into an equation. The equation is returned. 

Calc.prob() goes through all of the predictors that the user would like to include in the calculation, and adds their respective coefficients to a variable called log.odds. Log.odds is then put into the formula to calculate the probability of based off of the logistic-odds.

Lastly, the code includes my own palette, “pastel.grad”, that I created with the colorRampPalette() function. My own function, vis.palette(), can take in such a palette, and uses barplot() to create a barplot with each color taking up one bar.

In all of the functions that pass in a data frame, I include error handling to check if the input is valid using is.data.frame(). If it is not valid, then the program will stop execution and print an error message. Otherwise, the function will perform the transformations and return the updated data frame.


## Strengths and Weaknesses
Some of the strengths in my program include that it emphasizes modularity, flexibility, and readability. Each function addresses a specific task, such as cleaning column names, displaying a color palette, or dynamically adding conditional columns. By incorporating parameters, my code accommodates various user needs. Furthermore, error handling improves reliability by validating inputs. 

However, the code exhibits some inefficiencies. Iterative approaches, such as the use of loops in scale.vals() and conditional.col(), could be slower for larger datasets. Another area for improvement is error feedback and edge-case handling. While I do include error handling, they could be more specific, such as reporting the class of an invalid input or identifying conflicting conditions. My package also lacks unit tests for edge cases such as missing values, overlapping conditions, or unusual column names. Integrating more thorough testing and explicit handling of such cases would improve my code’s robustness.


## Conclusion
This package balances modularity, flexibility, and simplicity, enabling users to perform essential data cleaning and transformation tasks efficiently. Its functions integrate seamlessly into workflows, offering flexibility with parameters and error handling. Nevertheless, there are areas for further improvement, such as optimizing performance for large datasets and enhancing robustness by addressing edge cases. Despite these limitations, the package achieves its goal of simplifying data preparation and analysis. Future iterations could focus on improving efficiency, refining error feedback, and implementing unit tests to ensure reliability.
