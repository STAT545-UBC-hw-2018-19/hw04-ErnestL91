# HW-04 by Ernest  Lai

HW-04 looks at data reshaping and joining prompts. This repository contains the the project file, R-markdown file, github markdown file, the .csv excel files for the join prompt, and the folder containing the images for the markdown file. For more details on the assignment, please click [**here**](http://stat545.com/Classroom/assignments/hw04/hw04.html). 


**The markdown file explores the following:**

**1)** Data reshaping analysis: The [**gapminder**](https://cran.r-project.org/web/packages/gapminder/index.html) dataset was used, and I reshaped the gapminder file to provide a tibble with one row per year and columns for life expectancy for five countries. This new data shape was used to do a scatterplot looking at life expectancy between Canada and South Africa.

**Image files used:** [**Figure 1**](https://github.com/STAT545-UBC-students/hw04-ErnestL91/blob/master/hw-04_gapminder_files/figure-markdown_strict/untidy%20comparison%20scatter-1.png), [**Figure 2**](https://github.com/STAT545-UBC-students/hw04-ErnestL91/blob/master/hw-04_gapminder_files/figure-markdown_strict/tidy%20comparison%20scatter-1.png)


**2)** For the join prompt, I chose to focus my topic on soccer. Two .csv excel files were used:

[**Player Data**](https://github.com/STAT545-UBC-students/hw04-ErnestL91/blob/master/player_data.csv): Looks at globally known soccer players, their nationality and the clubs they play for. To offer variety, players from clubs from all around the world were chosen for this dataset.

[**Team Data**](https://github.com/STAT545-UBC-students/hw04-ErnestL91/blob/master/team_data.csv): Looks at globally known soccer clubs around the world, the league they're in, and their country location.

A multitude of join functions were used, matching the two datasets by the **Club** variable.


Assignment files:

[**.md**](https://github.com/STAT545-UBC-students/hw04-ErnestL91/blob/master/hw-04_gapminder.md)

[**.Rmd**](https://github.com/STAT545-UBC-students/hw04-ErnestL91/blob/master/hw-04_gapminder.Rmd)

[**Player Data**](https://github.com/STAT545-UBC-students/hw04-ErnestL91/blob/master/player_data.csv)

[**Team Data**](https://github.com/STAT545-UBC-students/hw04-ErnestL91/blob/master/team_data.csv)

[**Project File**](https://github.com/STAT545-UBC-students/hw04-ErnestL91/blob/master/Assignments.Rproj)
