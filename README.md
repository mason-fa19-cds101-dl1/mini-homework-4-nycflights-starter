# Mini-homework 4: Flights of New York

- [Mini-homework 4: Flights of New York](#mini-homework-4-flights-of-new-york)
  - [Instructions](#instructions)
  - [About the dataset](#about-the-dataset)
  - [Exercises](#exercises)
  - [How to submit](#how-to-submit)
  - [Cheatsheets](#cheatsheets)

## Instructions

Obtain the GitHub repository you will use to complete the mini-homework, which contains a starter file named `flights_of_new_york.Rmd`.
This mini-homework will help you become more familiar with manipulating datasets using R by guiding you through some examples.
Read the [About the dataset](#about-the-dataset) section to get some background information on the dataset that you'll be working with.
Each of the below [exercises](#exercises) are to be completed in the provided spaces within your starter file `flights_of_new_york.Rmd`.
Then, when you're ready to submit, follow the directions in the **[How to submit](#how-to-submit)** section below.

## About the dataset

The dataset we are working with in this mini-homework is the `flights` dataset that is loaded via the `nycflights13` R package.
This dataset contains on-time data for all flights that departed from a New York City airport (airport codes: JFK, LGA, EWR) in 2013.

## Exercises

Start working on these exercises **after** you have successfully cloned your repository into RStudio Server.

1. As always, you should inspect a new dataset to become more familiar with what it contains.
    
    First, run the setup code block in the `flights_of_new_york.Rmd` file.
    This will install and load the required packages.

    Then type the following command **in your *Console* window** (not in the R Markdown file):

    ```r
    View(flights)
    ```

    Since the `flights` dataset is loaded as an R package via the `library(nycflights13)` in your setup code block, additional documentation is available.
    To read more about the dataset, type the following command **in your *Console* window**:

    ```r
    ?flights
    ```

    Use these resources to answer these initial questions.

    1.  How many rows and columns does this dataset have?

    2.  What does a single row in this dataset represent?

    3.  What is the difference between the information contained in the `dep_time` and `sched_dep_time` columns?

    4.  Which columns contain information about dates and times?

    5.  Airplanes are reused across many different flights.
        Which columns would be helpful to use in identifying individual airplanes?

3. Let's start with the `select()` function.
    The `select()` function *selects* columns from a dataset, which is useful when you're working with a dataset that contains dozens of variables.
    Try running the following code:

    ```r
    flights %>%
      select(year, month)
    ```

    Based on the output, explain what happens when you run this command.

    > #### What does `%>%` mean?
    >
    > The strange looking symbol `%>%` is called the **pipe**, and it is a handy way to pass a dataset through a chain of commands.
    > All examples going forward will use the pipe whenever possible.
    

4. There are multiple ways to specify columns with the `select()` command.
    To demonstrate this, copy the code you wrote in **Exercise 2** and replace `select(year, month)` with `select(year:day)`.
    What does the colon `:` do?

5. The sort operation is a common and indispensible operation for organizing data, and the function from `dplyr` that allows us to sort is called `arrange()`.
    `arrange()` sorts columns with textual data (`chr` data type) into alphabetical order and sorts numerical data into numerical order.

    Try running the following code:

    ```r
    flights %>%
      arrange(month, day)
    ```

    Based on the output, does it look like both the `month` and `day` columns were sorted?
    Which column was sorted first?
    What happens if you reverse the order you specify the columns in `arrange()`?

    > #### Important
    >
    > Students often find that there is a _base R_ function named `sort()` as well, leading to confusion later in the course.
    > Using `sort()` the same way you use `arrange()` will give you errors.
    > For the duration of course **we will always prefer to use the `tidyverse` version of a function**, so always use `arrange()` and pretend `sort()` doesn’t exist.

6.  By default, `arrange()` will sort data in ascending order.
    The function `desc()` can be used to sort in descending order.
    For example, to sort by months in reverse order, we would use `arrange(desc(month))`
    
    Let's use it to answer a simple question about the dataset: *what flight experienced the longest departure delay?*
    Identify the column that gives information about flight departure delays, and then use `arrange()` in combination with `desc()` to sort the `flights` dataset to find the flight with the longest departure delay.

7.  Let's now try an example that uses the `mutate()` function, which is a little more complex.
    `mutate()` lets us **transform** a dataset by applying the same operation to each row in the dataset and appending the results as a *new* column.
    More simply, this is how you can add, subtract, multiply, and divide different two or more columns against each other.
    As an example, run the following command to calculate the average speed for each flight in miles per hour:

    ```r
    flights %>%
      mutate(
        average_speed = distance / (air_time * 60)
      )
    ```

    Where does the new column you just computed show up in the dataset and what is the name of this new column?
    What part of the code is controlling the name of the new column?

8.  We are not limited to only one input at a time in `mutate()`.
    As long as we separate each input by a comma, we can put as many inputs as we want in the `mutate()` function!
    As an example, let's take the `dep_time` column, which gives the clock time as an integer (for example, 11:00am is 1100) and separate it into an hours column and minutes column:

    ```r
    flights %>%
      mutate(
        dep_time_hour = dep_time %/% 100,
        dep_time_minute = dep_time %% 100
      )
    ```

    Next, copy and paste this code into a new code block, add a second comma, and then add a third line.
    **Have this third line use the `dep_time_hour` and `dep_time_minute` columns to compute the number of minutes since midnight.**
    Name this third column `dep_time_minutes_midnight`.

    > #### What do `%/%` and `%%` mean?
    >
    > The operators `%/%` and `%%` let you do **modular arithmetic.**
    > The symbol `%/%` means *integer division* and `%%` means *remainder*.
    > As a quick example, you might remember learning about proper and improper fractions in math class.
    $\dfrac{9}{4}$ is an example of an improper fraction.
    > To convert it into a proper fraction, first we need to figure out how many times 4 goes into 9.
    > We can easily compute this with integer division:
    >
    > ```{r integer-division-example, comment = "#"}
    > 9 %/% 4
    > ```
    >
    > We also need to know the remainder, or how much is left  over after dividing 4 into 9.
    >
    > ```{r remainder-example, comment = "#"}
    > 9 %% 4
    > ```
    >
    > So the proper fraction representation of $\dfrac{9}{4}$ is $2 \frac{1}{4}$.
    

9. Next we consider the `filter()` function, which provides a ruled-based way to keep a subset of rows and remove the rest.
    Here we just consider rules that are simple comparisons, which involve the symbols:

    *   `>`: greater than
    *   `>=`: greater than or equal to
    *   `<`: less than
    *   `<=`: less than or equal to
    *   `!=`: not equal
    *   `==`: equal

    Give `filter()` a try by running the following two code blocks:

    ```r
    flights %>%
      filter(
        arr_delay < 0
      )
    ```

    ```r
    flights %>%
      filter(
        carrier == "UA"
      )
    ```

    After running the above code, figure out how to combine the two examples in one `filter()` function (**hint:** it resembles what you did with `mutate()`).
    This will tell you all the flights operated by United Airlines (UA) that arrived early.

10. It is common to want to summarize the information contained within a dataset, such as computing sums and averages, or counting how many data points belong to different groups.
    This is called data aggregation, as it **aggregates** many data points together and uses them to compute a cetain quantity.
    We perform data aggregation in R by using the commands `group_by()` and `summarize()`, which frequently show up as a pair.

    The `group_by()` command is applied to one or more columns, and allows you create groups that share common values in a column of categorical data.
    The `summarize()` command can then be used to compute quantities like averages **within** those groups.
    It will help to see an example.
    Run the following code, which calculates the mean (average) arrival delay for each airline carrier:

    ```r
    flights %>%
      group_by(carrier) %>%
      summarize(
        average_arr_delay = mean(arr_delay, na.rm = TRUE)
      )
    ```

    Which airline carrier had the longest arrival delays on average?
    Which airline carrier had the shortest arrival delays on average?

## How to submit

To submit your mini-homework, follow the two steps below.
Your homework will be graded for credit **after** you've completed both steps!

1.  Save, commit, and push your completed R Markdown file so that everything is synchronized to GitHub.
    If you do this right, then you will be able to view your completed file on the GitHub website.

2.  Knit your R Markdown document to the PDF format, export (download) the PDF file from RStudio Server, and then upload it to *Mini-homework 4* posting on Blackboard.

## Cheatsheets

You are encouraged to review and keep the following cheatsheets handy while working on this mini-homework:

*   [dplyr cheatsheet][dplyr-cheatsheet]

*   [ggplot2 cheatsheet][ggplot2-cheatsheet]

*   [RStudio cheatsheet][rstudio-cheatsheet]

*   [RMarkdown cheatsheet][rmarkdown-cheatsheet]

*   [RMarkdown reference][rmarkdown-reference]

[dplyr-cheatsheet]:     https://github.com/rstudio/cheatsheets/raw/master/data-transformation.pdf
[ggplot2-cheatsheet]:   https://github.com/rstudio/cheatsheets/raw/master/data-visualization-2.1.pdf
[rstudio-cheatsheet]:   https://github.com/rstudio/cheatsheets/raw/master/rstudio-ide.pdf
[rmarkdown-reference]:  https://www.rstudio.com/wp-content/uploads/2015/03/rmarkdown-reference.pdf
[rmarkdown-cheatsheet]: https://github.com/rstudio/cheatsheets/raw/master/rmarkdown-2.0.pdf
