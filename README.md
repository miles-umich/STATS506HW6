Problem Set #06
Statistics 506
Due: Dec 5, 11:59am on Canvas

Instructions
This problem set consists of a single problem. It is worth half-credit compared to other problem sets.
Review the attribution of sources discussions.
Submit the output of your Quarto document on Canvas. The output should include a link to your GitHub repository for this assignment.
Unless otherwise explicitly stated, all problems should be solved in R.
Your output file should include all code required to solve the problem. Code folding may be useful to make the output more readable.
Use a consistent and readable code style for your code. Lack of a consistent and readable code style will negatively affect your grade.
Some of these exercises may require you to use commands or techniques that were not covered in class or in the course notes. You can use the web as needed to identify appropriate approaches. Part of the purpose of these exercises is for you to learn to be resourceful and self sufficient. Questions are welcome at all times, but please make an attempt to locate relevant information yourself first.
Be sure to properly document any functions you write using roxygen, and add comments as appropriate to make it clear what you are doing.
If submitting an HTML file, please make sure to make it self-contained.
Special Instruction: This problem set involves some slow-running code. Regardless of your speed at writing the code, this one may take longer than usual due simply to compile time. Please start this problem set early, to give yourself enough time to compile it.

Problem 1 - Rcpp
In the notes, we defined a C_mean function. Using this as a template, implement a C_moment function that returns the kth central moment. Generate a vector of moderate length and show that you are able to replicate the results of e1071::moment.

Notes & Hints:

Be cognizant of your scaling factor.
Be sure to look at the arguments of e1071::moment.
Problem 2 - Expanding on waldCI
Write a class bootstrapWaldCI that produces a CI using bootstrap, similar to waldCI. It should inherit from waldCI using either the version from the solutions or your version - in either case, include the relevant code in this homework by using source() on a file containing the waldCI code.

The booststrapWaldCI constructor should take in a data set and a function that returns a scalar. E.g.

makeBootstrapCI(function(x) mean(x$myvar),
                data = mydata,
                reps = 100)

Add an optional level argument and an optional compute argument that accepts either “serial” (using no parallel processing), “parallel” (using either forks or sockets from parallel).

The constructor should carry out the bootstrap using the requested compute approach. Most functions can be inherited from waldCI, but add the additional function rebootstrap that performs a new bootstrap. rebootstrap should only take in one argument of a bootstrapWaldCI object.

Note: Be careful with inheritance and S4:

The @.Data slot only applies if the child class contains an S3 class; if it contains S4, it simply adds the childs slots alongside the parent slots.
Slot names should not be re-used unless you’re explicltly overwriting existing slots.
Show your code works by executing the following:

ci1 <- makeBootstrapCI(function(x) mean(x$y),
                       ggplot2::diamonds,
                       reps = 1000)
ci1
rebootstrap(ci1)

Compare and comment on the performance of the two compute methods.

Write a function called dispCoef that takes in data (based upon mtcars; it must take in a generic data for the bootstrap) and fits the model: mpg ~     cyl + disp + wt. It should return the coefficient associated with disp. Execute the following:

ci2 <- makeBootstrapCI(dispCoef,
                       mtcars,
                       reps = 1000)
ci2
rebootstrap(ci2)

Compare and comment on the performance of the two compute methods.

Problem 3 - Large data
Generate artificial data by running the code in this script. Do not include this script in your submitted PDF; either use source() or save/load to get the data into R.

Fit one model per country. Fit a mixed effects logistic regression model, predicting course completion based upon prior GPA, number of forum posts, number of quiz attempts, and a random effect for device type. Standardize the predictors within each country. Generate some sort of visualization of the estimated coefficients for number of forum posts in each country.

Report the running time (from system.time) for each of the 6 models.

Devise an approach that minimizes the running time of your script. Report the running time of your entire script (running models and estimating coefficients; no need for a new plot). Do not use a different package for the models. Show that the results match those from part a).

Problem 4 - data.table
Note: This is problem 2 from the last problem set that was deferred to this problem set.

Repeat problem set 4, question 2, using data.table.

You can make the same or different decision as you did last time. You can also use or ignore the decisions I made in the solutions. As before, there is no “correct” answer (including those in the problem set 4 solutions).
