R Markdown
----------

This is the R Markdown document for hw-04, which is a data wrangling
exercise of the **gapminder** dataset. The focus of this assignment
would be on data reshaping (and relationship to aggregation) and data
join.

For simple reshaping, gather() and spread() from
[*tidyr*](https://tidyr.tidyverse.org/) - to help a more tidier dataset.
For data joining, there are two data sources and info from both datasets
are needed - hence a multitude of join prompts will be used to c ombine
info from both datasets into a single new object.

All of the functions used for the data reshaping and data join will be
available under the [*tidyverse*](https://www.tidyverse.org/packages/)
package. For full information about hw-04, please visit
[here](http://stat545.com/Classroom/assignments/hw04/hw04.html).

<br/>

Open the tidyverse and gapminder Package
----------------------------------------

As usual, we will begin with loading the packages needed for the
analysis (tidyverse for functions and gapminder as the dataset of
interest). If these packages are being used for the first time on your
R-Studio client, then the packages will need to be installed first prior
to being loaded.

    # If loading tidyverse and gapminder for first time, then run the install.packages function below before loading the library.

    # install.packages('tidyverse')
    # install.packages('gapminder')

    # Load tidyverse and gapminder
    library(tidyverse)
    library(gapminder)

<br/>

Data Reshaping Prompt
---------------------

For the data reshaping exercise, I will complete **Activity 2: Making a
tibble with one row per year and columns for life expectancy for two or
more countries**. The newly reshaped data frame will be put in table
form via **knitr::kable()**. Lastly, a scatterplot comparing life
expectancies from one country to another will be generated from the new
reshaped data set.

<br/>

### Reshaping the Data + Tabular Presentation

We will first assign the regular gapminder dataset as **gm\_untidy**,
work on reshaping this object to show life expectancies by year for 5
countries (1 for each geographic area according to the gapminder
dataset). We will keep year, lifeExp, and country as these are the
variables of interest we would wish to focus on.

For this activity, there were two main choices with how we wanted to
reshape the data: *gather()* or *spread()*. In this scenario, we are
taking a specific variable (country) and trying to present life
expectancy at each timepoint/every 5 years. Since we are using a
categorical variable and creating new columns based on the countries we
are interested in, we have to **spread** the data.

In this scenario, we are trying to take the untidy data and create wider
data frame - hence we take two important columns: country, which is our
key variable to expand on, and life expectancy, our value which we want
to present for each country of interest for all timepoints. In other
words, we are "spreading" each unique country in our country variable
into their own columns and presenting the life expectancy value for each
of their timepoint.

The gather function will not work in this scenario because it takes
multiple columns from the untidy data and puts them into key-value
pairs, making our new data frame longer rather than stretching the data
frame.

After reshaping our data using spread(), the results will be published
on a table.

    # Set gapminder as a data frame object
    gm_untidy <- gapminder

    # Set the data frame as a tibble (optional, as gm_untidy.1 is already a data frame). It is good practice to use tibble to create a data frame as it retains the input type for each variable.
    tibble <- as_tibble(gm_untidy)

    # Pipe tibble through df modification and reshaping functions, then present the end result in a table (see comments for explanation of each function). Store new reshaped data frame as gm_tidy, for plotting purposes later.
    gm_tidy <- tibble %>%
      
    # Capitalize year in our dataset, to be used in table for presenting reshaped data  
        rename(Year = year) %>%
      
    # select variables of interest from gm_untidy.1
        select("country", "Year", "lifeExp") %>% 
      
    # select countries of interest from the gapminder dataset
        filter(country %in% c("Canada", "France", "Japan", "South Africa", "New Zealand")) %>%
      
    # take the country (key) column, spread all unique countries into its own column, then populate life expectancy (value) with respect to the specified country  
        spread(key = "country", value = "lifeExp") 

    # present newly reshaped data frame into a table, with additional formatting (preserve row and column labels)
    gm_tidy %>%
    knitr::kable(caption = "This table summarizes the life expectancy every 4 years for Canada, France, Japan, New Zealand, and South Africa")

<table>
<caption>This table summarizes the life expectancy every 4 years for Canada, France, Japan, New Zealand, and South Africa</caption>
<thead>
<tr class="header">
<th align="right">Year</th>
<th align="right">Canada</th>
<th align="right">France</th>
<th align="right">Japan</th>
<th align="right">New Zealand</th>
<th align="right">South Africa</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="right">1952</td>
<td align="right">68.750</td>
<td align="right">67.410</td>
<td align="right">63.030</td>
<td align="right">69.390</td>
<td align="right">45.009</td>
</tr>
<tr class="even">
<td align="right">1957</td>
<td align="right">69.960</td>
<td align="right">68.930</td>
<td align="right">65.500</td>
<td align="right">70.260</td>
<td align="right">47.985</td>
</tr>
<tr class="odd">
<td align="right">1962</td>
<td align="right">71.300</td>
<td align="right">70.510</td>
<td align="right">68.730</td>
<td align="right">71.240</td>
<td align="right">49.951</td>
</tr>
<tr class="even">
<td align="right">1967</td>
<td align="right">72.130</td>
<td align="right">71.550</td>
<td align="right">71.430</td>
<td align="right">71.520</td>
<td align="right">51.927</td>
</tr>
<tr class="odd">
<td align="right">1972</td>
<td align="right">72.880</td>
<td align="right">72.380</td>
<td align="right">73.420</td>
<td align="right">71.890</td>
<td align="right">53.696</td>
</tr>
<tr class="even">
<td align="right">1977</td>
<td align="right">74.210</td>
<td align="right">73.830</td>
<td align="right">75.380</td>
<td align="right">72.220</td>
<td align="right">55.527</td>
</tr>
<tr class="odd">
<td align="right">1982</td>
<td align="right">75.760</td>
<td align="right">74.890</td>
<td align="right">77.110</td>
<td align="right">73.840</td>
<td align="right">58.161</td>
</tr>
<tr class="even">
<td align="right">1987</td>
<td align="right">76.860</td>
<td align="right">76.340</td>
<td align="right">78.670</td>
<td align="right">74.320</td>
<td align="right">60.834</td>
</tr>
<tr class="odd">
<td align="right">1992</td>
<td align="right">77.950</td>
<td align="right">77.460</td>
<td align="right">79.360</td>
<td align="right">76.330</td>
<td align="right">61.888</td>
</tr>
<tr class="even">
<td align="right">1997</td>
<td align="right">78.610</td>
<td align="right">78.640</td>
<td align="right">80.690</td>
<td align="right">77.550</td>
<td align="right">60.236</td>
</tr>
<tr class="odd">
<td align="right">2002</td>
<td align="right">79.770</td>
<td align="right">79.590</td>
<td align="right">82.000</td>
<td align="right">79.110</td>
<td align="right">53.365</td>
</tr>
<tr class="even">
<td align="right">2007</td>
<td align="right">80.653</td>
<td align="right">80.657</td>
<td align="right">82.603</td>
<td align="right">80.204</td>
<td align="right">49.339</td>
</tr>
<tr class="odd">
<td align="right"><br/></td>
<td align="right"></td>
<td align="right"></td>
<td align="right"></td>
<td align="right"></td>
<td align="right"></td>
</tr>
</tbody>
</table>

For the most part, the 5 countries of interest in our data analysis show
improvement in life expectancy at each subsequent timepoint/every 5
years. The only exception is SOuth Africa, where there is a small
decline from 1992 to 1997, but the saw sharper decline for the 2
subsequent timepoints after 1997. Life expectancy is generally in the
same range between Canada, Japan, France, and New Zealand, but it is
noticeable lower comparatively for South Africa.

The next section will show a scatterplot of life expectancy across all
timepoints for the 5 countries. This will allow to make life expectancy
comparisons between countries based on graphical observations.

<br/>

### Life Expectancy Plots

Two plots will be presented: An individual scatter plot of life
expectancy across all timepoints for each country from the untidy data
set, and a scatterplot comparing Canada's life expectancy to South
Africa and looking at their trend/relationship.

    # Start a scatterplot with the untidy data first
    tibble %>%
      
    # select variables of interest from gm_untidy.1
        select("country", "year", "lifeExp") %>% 
      
    # select countries of interest from the gapminder dataset
        filter(country %in% c("Canada", "France", "Japan", "South Africa", "New Zealand")) %>%

    # Plot life expectancies of all countries on one scatterplot, colour code points by country
      ggplot(aes(year, lifeExp), group = country) + # plot life expectancy against year
      geom_point(aes(colour = country), alpha = 1) + # lifeExp for each country every 5 years
      geom_line(aes(colour = country)) + # plot line through points, colour coded by country

    # Set axis limits, add title (main and axis), and descriptions
      ylim(40, 85) + # limit for y-axis
      labs(title = "Comparison of Life Expectancy between all 5 countries", 
           y = "Life Expectancy (in Years)", x ="Year",
           caption = "Figure 1. Life expectancy every 5 years between South Africa and Canada",
           colour = "Country") + 
      theme_bw() # give a white background to the graph

![](hw-04_gapminder_files/figure-markdown_strict/untidy%20comparison%20scatter-1.png)
<br/>

The visual plot from the untidy dataset confirm our observations from
the tabular data. Canada, France, Japan, and New Zealand have similar
life expectancies across all timepoints and seem to trend towards and
improvement with every subsequent timepoint. Japan did have a lower life
expectancy than the three other countries to start, but quickly caught
up in 1967. South Africa in comparison has noticeably lower life
expectancy than Canada, France, Japan, and New Zealand. The life
expectancy for South Africa improves with each subsequent timepoint, but
saw a sharp decline from 1997 to 2007.

For the next plot, we will use the reshaped data set to plot life
expectancies of South Africa against the life expectancies of Canada
across all time points. I chose Canada because I have lived here for
almost my whole life, and I am comparing it to South Africa because this
country had a noticeably different tracjectory in life expectancy across
all timepoints compared to Japan, New Zealand and France.

    # Create scatter plot, with Canada and Africa on the x- and y-axis respectively. 
    gm_tidy %>%

    # Convert Year to factor for this exercise, to give contrasting colors for each data point
      mutate(Year = factor(Year, levels = c(1952, 1957, 1962, 1967, 1972, 1977, 1982, 1987, 1992,
                                            1997, 2002, 2007))) %>%
      
    # Rename South Africa (empty space not allowed when selecting variables for analysis)
      rename(South_Africa = "South Africa") %>%
      
    # Plot life expectancies of South Africa against Canada
      ggplot(aes(Canada, South_Africa), group = Year) + # Plot Canada against South Africa
      geom_point(aes(colour = Year), alpha = 1) + # plot lifeExp at each year/timepoint
      geom_line(alpha = 0.2) + # plot line through all points in order of timepoint

    # Set axis limits, add title (main and axis), and descriptions
      xlim(65, 85) + # limit for x-axis
      ylim(45, 65) + # limit for y-axis
      labs(title = "Comparison of Life Expectancy between South Africa and Canada", 
           y = "South Africa", x ="Canada",
           caption = "Figure 2. Life expectancy every 5 years between South Africa and Canada") + 
      theme_bw() # give a white background to the graph

![](hw-04_gapminder_files/figure-markdown_strict/tidy%20comparison%20scatter-1.png)
<br/>

This plot is a bit different from the first plot, as this plot allows
the comparison between the rate of change in life expectancy between two
countries (South Africa and Canada in this example). Even though South
Africa has a noticeably lower life expectancy at the start compared to
Canada, the life expectancies of South Africa at subsequent timepoints
are improving at a faster rate than Canada - as evident by the steep
positive slope.

One would expect South Africa to eventually catch up to the other
countries in a couple of decades if the trend continued, but the sharp
decrease as mentioned earlier from 1997 to 2007 changed the course of
the line plot. During this time period, the slope is negative because
Canada's life expectancy was still improving at a steady slow rate but
South Africa saw their life expectancy worsen in comparison to previous
timepoints.

Join Prompts
------------

For this exercise, I will focus on creating a cheatsheet patterned after
[Jenny's tutorial](http://stat545.com/bit001_dplyr-cheatsheet.html) on
join prompts. My specific topics on this will be on soccer players,
their nationality, the club they play for and the league name + country
of the club they are in.

### Load .csv Files Regarding Player and Team Data

There will be two spreadsheets for this section: One for player data and
one for team data. The player data will consist of player name,
nationality and current club. The team data will contain details on
clubs, such as the league they are in and its nationality. Lets load the
data:

    # Load tidyverse library if you haven't already
    # library(tidyverse)

    # Load player data and team data
    library(readr) # load library for read_csv function

    pdata <- read_csv("player_data.csv", skip = 0) # read player data, don't skip any rows
    tdata <- read_csv("team_data.csv", skip = 0) # read team data, don't skip any rows

We loaded the tidyverse package earlier, the tidyverse package contains
**dplyr** which provides the join functions we will be using. The
**readr** package allows us to use the *read\_csv()* function to upload
the csv into our R-Studio environment.

### The Data

Working with two dataframes **pdata (x)** and **tdata (y)**, we will use
*left\_join*, *right\_join*, *inner\_join*, *semi\_join*, *full\_join*,
and *anti\_join*. The player data consists of players playing all around
the world, from clubs in all continents. The club data consists of teams
whom are very famous and globally known.

### left\_join(x,y)

    left_join(pdata, tdata, by = "Club") %>% # join by variable 'Club'
    knitr::kable(caption = "Left join between pdata (x) and tdata (y)")

<table>
<caption>Left join between pdata (x) and tdata (y)</caption>
<thead>
<tr class="header">
<th align="left">First_Name</th>
<th align="left">Last_Name</th>
<th align="left">Club</th>
<th align="left">National_Team</th>
<th align="left">League</th>
<th align="left">League_Country</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Ahmed</td>
<td align="left">Musa</td>
<td align="left">Al-Nassr FC</td>
<td align="left">Nigeria</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Alphonso</td>
<td align="left">Davies</td>
<td align="left">Vancouver Whitecaps</td>
<td align="left">Canada</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Anderson</td>
<td align="left">Talisca</td>
<td align="left">Guangzhou Evergrande</td>
<td align="left">Brazil</td>
<td align="left">Chinese Super League</td>
<td align="left">China</td>
</tr>
<tr class="even">
<td align="left">Andres</td>
<td align="left">Iniesta</td>
<td align="left">FC Barcelona</td>
<td align="left">Spain</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Anice</td>
<td align="left">Badri</td>
<td align="left">Esperance Tunis</td>
<td align="left">Tunisia</td>
<td align="left">Tunisian Ligue Professionnelle 1</td>
<td align="left">Tunisia</td>
</tr>
<tr class="even">
<td align="left">Atsuto</td>
<td align="left">Uchida</td>
<td align="left">Kashima Antlers</td>
<td align="left">Japan</td>
<td align="left">J-League 1</td>
<td align="left">Japan</td>
</tr>
<tr class="odd">
<td align="left">Bafetimbi</td>
<td align="left">Gomis</td>
<td align="left">Al-Hilal FC</td>
<td align="left">France</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Balazs</td>
<td align="left">Dzusudzsak</td>
<td align="left">Al-Ittihad Kalba SC</td>
<td align="left">Hungary</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="odd">
<td align="left">Carlos</td>
<td align="left">Eduardo</td>
<td align="left">Al-Hilal FC</td>
<td align="left">Brazil</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Carlos</td>
<td align="left">Tevez</td>
<td align="left">Boca Juniors</td>
<td align="left">Argentina</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">Cristiano</td>
<td align="left">Ronaldo</td>
<td align="left">Juventus</td>
<td align="left">Portugal</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="even">
<td align="left">Dario</td>
<td align="left">Benedetto</td>
<td align="left">Boca Juniors</td>
<td align="left">Argentina</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">David</td>
<td align="left">de Gea</td>
<td align="left">Manchester United</td>
<td align="left">Spain</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="even">
<td align="left">Diego</td>
<td align="left">Alves</td>
<td align="left">Flamengo</td>
<td align="left">Brazil</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="odd">
<td align="left">Diego</td>
<td align="left">Valeri</td>
<td align="left">Portland Timbers</td>
<td align="left">Argentina</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Diego</td>
<td align="left">Chara</td>
<td align="left">Portland Timbers</td>
<td align="left">Colombia</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Ezequiel</td>
<td align="left">Barco</td>
<td align="left">Atlanta United</td>
<td align="left">Argentina</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Ferjani</td>
<td align="left">Sassi</td>
<td align="left">Zamalek SC</td>
<td align="left">Tunisia</td>
<td align="left">Egyptian Premier League</td>
<td align="left">Egypt</td>
</tr>
<tr class="odd">
<td align="left">Gabriel</td>
<td align="left">Barbosa</td>
<td align="left">Santos FC</td>
<td align="left">Brazil</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Gareth</td>
<td align="left">Bale</td>
<td align="left">Real Madrid</td>
<td align="left">Wales</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Gianluigi</td>
<td align="left">Buffon</td>
<td align="left">Paris St. Germain</td>
<td align="left">Italy</td>
<td align="left">Ligue 1</td>
<td align="left">France</td>
</tr>
<tr class="even">
<td align="left">Giorgio</td>
<td align="left">Chiellini</td>
<td align="left">Juventus</td>
<td align="left">Italy</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="odd">
<td align="left">Jay</td>
<td align="left">Bothroyd</td>
<td align="left">Hokkaido Consadole Sapporo</td>
<td align="left">England</td>
<td align="left">J-League 1</td>
<td align="left">Japan</td>
</tr>
<tr class="even">
<td align="left">Josef</td>
<td align="left">Martinez</td>
<td align="left">Atlanta United</td>
<td align="left">Venezuela</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Karim</td>
<td align="left">Benzema</td>
<td align="left">Real Madrid</td>
<td align="left">France</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="even">
<td align="left">Kei</td>
<td align="left">Kamara</td>
<td align="left">Vancouver Whitecaps</td>
<td align="left">Sierra Leone</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Keisuke</td>
<td align="left">Honda</td>
<td align="left">Melbourne Victory</td>
<td align="left">Japan</td>
<td align="left">A-League</td>
<td align="left">Australia</td>
</tr>
<tr class="even">
<td align="left">Kendall</td>
<td align="left">Waston</td>
<td align="left">Vancouver Whitecaps</td>
<td align="left">Costa Rica</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Kengo</td>
<td align="left">Nakamura</td>
<td align="left">Kawasaki Frontale</td>
<td align="left">Japan</td>
<td align="left">J-League 1</td>
<td align="left">Japan</td>
</tr>
<tr class="even">
<td align="left">Kylian</td>
<td align="left">Mbappe</td>
<td align="left">Paris St. Germain</td>
<td align="left">France</td>
<td align="left">Ligue 1</td>
<td align="left">France</td>
</tr>
<tr class="odd">
<td align="left">Lei</td>
<td align="left">Wu</td>
<td align="left">Shanghai SIPG</td>
<td align="left">China</td>
<td align="left">Chinese Super League</td>
<td align="left">China</td>
</tr>
<tr class="even">
<td align="left">Lionel</td>
<td align="left">Messi</td>
<td align="left">FC Barcelona</td>
<td align="left">Argentina</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Lisandro</td>
<td align="left">Lopez</td>
<td align="left">Racing Club</td>
<td align="left">Argentina</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
</tr>
<tr class="even">
<td align="left">Lucas</td>
<td align="left">Paqueta</td>
<td align="left">Flamengo</td>
<td align="left">Brazil</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="odd">
<td align="left">Lucas</td>
<td align="left">Lima</td>
<td align="left">Palmeiras</td>
<td align="left">Brazil</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Lucho</td>
<td align="left">Gonzalez</td>
<td align="left">Atletico Paranaense</td>
<td align="left">Argentina</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="odd">
<td align="left">Luis</td>
<td align="left">Suarez</td>
<td align="left">FC Barcelona</td>
<td align="left">Uruguay</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="even">
<td align="left">Luka</td>
<td align="left">Modric</td>
<td align="left">Real Madrid</td>
<td align="left">Croatia</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Marcus</td>
<td align="left">Berg</td>
<td align="left">Al-Ain FC</td>
<td align="left">Sweden</td>
<td align="left">Arabian Gulf League</td>
<td align="left">United Arab Emirates</td>
</tr>
<tr class="even">
<td align="left">Marcus</td>
<td align="left">Rashford</td>
<td align="left">Manchester United</td>
<td align="left">England</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="odd">
<td align="left">Miguel</td>
<td align="left">Almiron</td>
<td align="left">Atlanta United</td>
<td align="left">Paraguay</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Miguel</td>
<td align="left">Borja</td>
<td align="left">Palmeiras</td>
<td align="left">Colombia</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="odd">
<td align="left">Milos</td>
<td align="left">Ninkovic</td>
<td align="left">Sydney FC</td>
<td align="left">Serbia</td>
<td align="left">A-League</td>
<td align="left">Australia</td>
</tr>
<tr class="even">
<td align="left">Nana</td>
<td align="left">Kwesi</td>
<td align="left">TP Mazembe</td>
<td align="left">Ghana</td>
<td align="left">Linafoot</td>
<td align="left">Congo</td>
</tr>
<tr class="odd">
<td align="left">Nemanja</td>
<td align="left">Matic</td>
<td align="left">Manchester United</td>
<td align="left">Serbia</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="even">
<td align="left">Neymar</td>
<td align="left">da Silva</td>
<td align="left">Paris St. Germain</td>
<td align="left">Brazil</td>
<td align="left">Ligue 1</td>
<td align="left">France</td>
</tr>
<tr class="odd">
<td align="left">Nordin</td>
<td align="left">Amrabat</td>
<td align="left">Al-Nassr FC</td>
<td align="left">Morocco</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Omar</td>
<td align="left">Al Somah</td>
<td align="left">Al-Ahli Saudi FC</td>
<td align="left">Syria</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="odd">
<td align="left">Oscar</td>
<td align="left">dos Santos</td>
<td align="left">Shanghai SIPG</td>
<td align="left">Brazil</td>
<td align="left">Chinese Super League</td>
<td align="left">China</td>
</tr>
<tr class="even">
<td align="left">Paul</td>
<td align="left">Pogba</td>
<td align="left">Manchester United</td>
<td align="left">France</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="odd">
<td align="left">Paulo</td>
<td align="left">Diaz</td>
<td align="left">Al-Ahli Saudi FC</td>
<td align="left">Chile</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Paulo</td>
<td align="left">Dybala</td>
<td align="left">Juventus</td>
<td align="left">Argentina</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="odd">
<td align="left">Ricardo</td>
<td align="left">Goulart</td>
<td align="left">Guangzhou Evergrande</td>
<td align="left">Brazil</td>
<td align="left">Chinese Super League</td>
<td align="left">China</td>
</tr>
<tr class="even">
<td align="left">Salah</td>
<td align="left">Mohsen</td>
<td align="left">Al-Ahly SC</td>
<td align="left">Egypt</td>
<td align="left">Egyptian Premier League</td>
<td align="left">Egypt</td>
</tr>
<tr class="odd">
<td align="left">Sebastien</td>
<td align="left">Blanco</td>
<td align="left">Portland Timbers</td>
<td align="left">Argentina</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Sergio</td>
<td align="left">Ramos</td>
<td align="left">Real Madrid</td>
<td align="left">Spain</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Siem</td>
<td align="left">De Jong</td>
<td align="left">Sydney FC</td>
<td align="left">Netherlands</td>
<td align="left">A-League</td>
<td align="left">Australia</td>
</tr>
<tr class="even">
<td align="left">Walid</td>
<td align="left">El Karti</td>
<td align="left">Wydad Athletic Club</td>
<td align="left">Morocco</td>
<td align="left">Botola</td>
<td align="left">Morocco</td>
</tr>
<tr class="odd">
<td align="left">Xavi</td>
<td align="left">Hernandez</td>
<td align="left">Al Sadd SC</td>
<td align="left">Spain</td>
<td align="left">Qatar Stars League</td>
<td align="left">Qatar</td>
</tr>
<tr class="even">
<td align="left">Zlatan</td>
<td align="left">Ibrahimovic</td>
<td align="left">LA Galaxy</td>
<td align="left">Sweden</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
</tbody>
</table>

The **left\_join** function is a mutating join that returns all rows
from 'x' (pdata) and adds columns from 'y' (tdata). This function
matches pdata and tdata by the variable **Club**. If **Club** from pdata
and tdata match then the function will bind additional information from
tdata such as **League** and **League\_Country** to pdata. If there are
multiple matches between pdata and tdata by **Club**, all combination of
the matches are returned.The new table tells us the club each player
plays for, and the league and country the club is under.

We could also run left\_join function without specifying which variable
to match by, R is smart enough to fan out the identical column names
between pdata and tdata and match on those columns. For future join
functions, we will exclude the 'by' portion of the code - as we will be
using pdata and tdata only.

### right\_join(x,y)

    right_join(pdata, tdata) %>% # join by variable 'Club'
    knitr::kable(caption = "Right join between pdata (x) and tdata (y)")

    ## Joining, by = "Club"

<table>
<caption>Right join between pdata (x) and tdata (y)</caption>
<thead>
<tr class="header">
<th align="left">First_Name</th>
<th align="left">Last_Name</th>
<th align="left">Club</th>
<th align="left">National_Team</th>
<th align="left">League</th>
<th align="left">League_Country</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">AC Milan</td>
<td align="left">NA</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="even">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Ajax Amsterdam</td>
<td align="left">NA</td>
<td align="left">Eredivisie</td>
<td align="left">Netherlands</td>
</tr>
<tr class="odd">
<td align="left">Xavi</td>
<td align="left">Hernandez</td>
<td align="left">Al Sadd SC</td>
<td align="left">Spain</td>
<td align="left">Qatar Stars League</td>
<td align="left">Qatar</td>
</tr>
<tr class="even">
<td align="left">Omar</td>
<td align="left">Al Somah</td>
<td align="left">Al-Ahli Saudi FC</td>
<td align="left">Syria</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="odd">
<td align="left">Paulo</td>
<td align="left">Diaz</td>
<td align="left">Al-Ahli Saudi FC</td>
<td align="left">Chile</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Salah</td>
<td align="left">Mohsen</td>
<td align="left">Al-Ahly SC</td>
<td align="left">Egypt</td>
<td align="left">Egyptian Premier League</td>
<td align="left">Egypt</td>
</tr>
<tr class="odd">
<td align="left">Marcus</td>
<td align="left">Berg</td>
<td align="left">Al-Ain FC</td>
<td align="left">Sweden</td>
<td align="left">Arabian Gulf League</td>
<td align="left">United Arab Emirates</td>
</tr>
<tr class="even">
<td align="left">Bafetimbi</td>
<td align="left">Gomis</td>
<td align="left">Al-Hilal FC</td>
<td align="left">France</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="odd">
<td align="left">Carlos</td>
<td align="left">Eduardo</td>
<td align="left">Al-Hilal FC</td>
<td align="left">Brazil</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Balazs</td>
<td align="left">Dzusudzsak</td>
<td align="left">Al-Ittihad Kalba SC</td>
<td align="left">Hungary</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="odd">
<td align="left">Ahmed</td>
<td align="left">Musa</td>
<td align="left">Al-Nassr FC</td>
<td align="left">Nigeria</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Nordin</td>
<td align="left">Amrabat</td>
<td align="left">Al-Nassr FC</td>
<td align="left">Morocco</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="odd">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Arsenal FC</td>
<td align="left">NA</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="even">
<td align="left">Ezequiel</td>
<td align="left">Barco</td>
<td align="left">Atlanta United</td>
<td align="left">Argentina</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Josef</td>
<td align="left">Martinez</td>
<td align="left">Atlanta United</td>
<td align="left">Venezuela</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Miguel</td>
<td align="left">Almiron</td>
<td align="left">Atlanta United</td>
<td align="left">Paraguay</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Lucho</td>
<td align="left">Gonzalez</td>
<td align="left">Atletico Paranaense</td>
<td align="left">Argentina</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Bate Borisov</td>
<td align="left">NA</td>
<td align="left">Belarusian Premier League</td>
<td align="left">Belarus</td>
</tr>
<tr class="odd">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Bayern Munich</td>
<td align="left">NA</td>
<td align="left">Bundesliga</td>
<td align="left">Germany</td>
</tr>
<tr class="even">
<td align="left">Carlos</td>
<td align="left">Tevez</td>
<td align="left">Boca Juniors</td>
<td align="left">Argentina</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">Dario</td>
<td align="left">Benedetto</td>
<td align="left">Boca Juniors</td>
<td align="left">Argentina</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
</tr>
<tr class="even">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Club Brugge</td>
<td align="left">NA</td>
<td align="left">Belgian Pro League</td>
<td align="left">Belgium</td>
</tr>
<tr class="odd">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">CSKA Moscow</td>
<td align="left">NA</td>
<td align="left">Russian Premier League</td>
<td align="left">Russain</td>
</tr>
<tr class="even">
<td align="left">Anice</td>
<td align="left">Badri</td>
<td align="left">Esperance Tunis</td>
<td align="left">Tunisia</td>
<td align="left">Tunisian Ligue Professionnelle 1</td>
<td align="left">Tunisia</td>
</tr>
<tr class="odd">
<td align="left">Andres</td>
<td align="left">Iniesta</td>
<td align="left">FC Barcelona</td>
<td align="left">Spain</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="even">
<td align="left">Lionel</td>
<td align="left">Messi</td>
<td align="left">FC Barcelona</td>
<td align="left">Argentina</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Luis</td>
<td align="left">Suarez</td>
<td align="left">FC Barcelona</td>
<td align="left">Uruguay</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="even">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Feyenoord</td>
<td align="left">NA</td>
<td align="left">Eredivisie</td>
<td align="left">Netherlands</td>
</tr>
<tr class="odd">
<td align="left">Diego</td>
<td align="left">Alves</td>
<td align="left">Flamengo</td>
<td align="left">Brazil</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Lucas</td>
<td align="left">Paqueta</td>
<td align="left">Flamengo</td>
<td align="left">Brazil</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="odd">
<td align="left">Anderson</td>
<td align="left">Talisca</td>
<td align="left">Guangzhou Evergrande</td>
<td align="left">Brazil</td>
<td align="left">Chinese Super League</td>
<td align="left">China</td>
</tr>
<tr class="even">
<td align="left">Ricardo</td>
<td align="left">Goulart</td>
<td align="left">Guangzhou Evergrande</td>
<td align="left">Brazil</td>
<td align="left">Chinese Super League</td>
<td align="left">China</td>
</tr>
<tr class="odd">
<td align="left">Jay</td>
<td align="left">Bothroyd</td>
<td align="left">Hokkaido Consadole Sapporo</td>
<td align="left">England</td>
<td align="left">J-League 1</td>
<td align="left">Japan</td>
</tr>
<tr class="even">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Inter Milan</td>
<td align="left">NA</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="odd">
<td align="left">Cristiano</td>
<td align="left">Ronaldo</td>
<td align="left">Juventus</td>
<td align="left">Portugal</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="even">
<td align="left">Giorgio</td>
<td align="left">Chiellini</td>
<td align="left">Juventus</td>
<td align="left">Italy</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="odd">
<td align="left">Paulo</td>
<td align="left">Dybala</td>
<td align="left">Juventus</td>
<td align="left">Argentina</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="even">
<td align="left">Atsuto</td>
<td align="left">Uchida</td>
<td align="left">Kashima Antlers</td>
<td align="left">Japan</td>
<td align="left">J-League 1</td>
<td align="left">Japan</td>
</tr>
<tr class="odd">
<td align="left">Kengo</td>
<td align="left">Nakamura</td>
<td align="left">Kawasaki Frontale</td>
<td align="left">Japan</td>
<td align="left">J-League 1</td>
<td align="left">Japan</td>
</tr>
<tr class="even">
<td align="left">Zlatan</td>
<td align="left">Ibrahimovic</td>
<td align="left">LA Galaxy</td>
<td align="left">Sweden</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Manchester City</td>
<td align="left">NA</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="even">
<td align="left">David</td>
<td align="left">de Gea</td>
<td align="left">Manchester United</td>
<td align="left">Spain</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="odd">
<td align="left">Marcus</td>
<td align="left">Rashford</td>
<td align="left">Manchester United</td>
<td align="left">England</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="even">
<td align="left">Nemanja</td>
<td align="left">Matic</td>
<td align="left">Manchester United</td>
<td align="left">Serbia</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="odd">
<td align="left">Paul</td>
<td align="left">Pogba</td>
<td align="left">Manchester United</td>
<td align="left">France</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="even">
<td align="left">Keisuke</td>
<td align="left">Honda</td>
<td align="left">Melbourne Victory</td>
<td align="left">Japan</td>
<td align="left">A-League</td>
<td align="left">Australia</td>
</tr>
<tr class="odd">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Napoli SSC</td>
<td align="left">NA</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="even">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Olympique Lyonnais</td>
<td align="left">NA</td>
<td align="left">Ligue 1</td>
<td align="left">France</td>
</tr>
<tr class="odd">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Olympique Marseille</td>
<td align="left">NA</td>
<td align="left">Ligue 1</td>
<td align="left">France</td>
</tr>
<tr class="even">
<td align="left">Lucas</td>
<td align="left">Lima</td>
<td align="left">Palmeiras</td>
<td align="left">Brazil</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="odd">
<td align="left">Miguel</td>
<td align="left">Borja</td>
<td align="left">Palmeiras</td>
<td align="left">Colombia</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Gianluigi</td>
<td align="left">Buffon</td>
<td align="left">Paris St. Germain</td>
<td align="left">Italy</td>
<td align="left">Ligue 1</td>
<td align="left">France</td>
</tr>
<tr class="odd">
<td align="left">Kylian</td>
<td align="left">Mbappe</td>
<td align="left">Paris St. Germain</td>
<td align="left">France</td>
<td align="left">Ligue 1</td>
<td align="left">France</td>
</tr>
<tr class="even">
<td align="left">Neymar</td>
<td align="left">da Silva</td>
<td align="left">Paris St. Germain</td>
<td align="left">Brazil</td>
<td align="left">Ligue 1</td>
<td align="left">France</td>
</tr>
<tr class="odd">
<td align="left">Diego</td>
<td align="left">Valeri</td>
<td align="left">Portland Timbers</td>
<td align="left">Argentina</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Diego</td>
<td align="left">Chara</td>
<td align="left">Portland Timbers</td>
<td align="left">Colombia</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Sebastien</td>
<td align="left">Blanco</td>
<td align="left">Portland Timbers</td>
<td align="left">Argentina</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Lisandro</td>
<td align="left">Lopez</td>
<td align="left">Racing Club</td>
<td align="left">Argentina</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">Gareth</td>
<td align="left">Bale</td>
<td align="left">Real Madrid</td>
<td align="left">Wales</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="even">
<td align="left">Karim</td>
<td align="left">Benzema</td>
<td align="left">Real Madrid</td>
<td align="left">France</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Luka</td>
<td align="left">Modric</td>
<td align="left">Real Madrid</td>
<td align="left">Croatia</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="even">
<td align="left">Sergio</td>
<td align="left">Ramos</td>
<td align="left">Real Madrid</td>
<td align="left">Spain</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Gabriel</td>
<td align="left">Barbosa</td>
<td align="left">Santos FC</td>
<td align="left">Brazil</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Schalke 04</td>
<td align="left">NA</td>
<td align="left">Bundesliga</td>
<td align="left">Germany</td>
</tr>
<tr class="odd">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Seattle Sounders</td>
<td align="left">NA</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Sevilla</td>
<td align="left">NA</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Lei</td>
<td align="left">Wu</td>
<td align="left">Shanghai SIPG</td>
<td align="left">China</td>
<td align="left">Chinese Super League</td>
<td align="left">China</td>
</tr>
<tr class="even">
<td align="left">Oscar</td>
<td align="left">dos Santos</td>
<td align="left">Shanghai SIPG</td>
<td align="left">Brazil</td>
<td align="left">Chinese Super League</td>
<td align="left">China</td>
</tr>
<tr class="odd">
<td align="left">Milos</td>
<td align="left">Ninkovic</td>
<td align="left">Sydney FC</td>
<td align="left">Serbia</td>
<td align="left">A-League</td>
<td align="left">Australia</td>
</tr>
<tr class="even">
<td align="left">Siem</td>
<td align="left">De Jong</td>
<td align="left">Sydney FC</td>
<td align="left">Netherlands</td>
<td align="left">A-League</td>
<td align="left">Australia</td>
</tr>
<tr class="odd">
<td align="left">Nana</td>
<td align="left">Kwesi</td>
<td align="left">TP Mazembe</td>
<td align="left">Ghana</td>
<td align="left">Linafoot</td>
<td align="left">Congo</td>
</tr>
<tr class="even">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Valencia CF</td>
<td align="left">NA</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Alphonso</td>
<td align="left">Davies</td>
<td align="left">Vancouver Whitecaps</td>
<td align="left">Canada</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Kei</td>
<td align="left">Kamara</td>
<td align="left">Vancouver Whitecaps</td>
<td align="left">Sierra Leone</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Kendall</td>
<td align="left">Waston</td>
<td align="left">Vancouver Whitecaps</td>
<td align="left">Costa Rica</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Walid</td>
<td align="left">El Karti</td>
<td align="left">Wydad Athletic Club</td>
<td align="left">Morocco</td>
<td align="left">Botola</td>
<td align="left">Morocco</td>
</tr>
<tr class="odd">
<td align="left">Ferjani</td>
<td align="left">Sassi</td>
<td align="left">Zamalek SC</td>
<td align="left">Tunisia</td>
<td align="left">Egyptian Premier League</td>
<td align="left">Egypt</td>
</tr>
</tbody>
</table>

The **right\_join** function is a mutating join that returns all rows
from 'y' (pdata) and adds columns from 'x' (tdata). This function
matches pdata and tdata by the variable **Club**. If **Club** from pdata
and tdata match then the function will bind additional information from
pdata such as **First\_Name** and **Last\_Name** to pdata. The new table
tells us all club names, and adds the players from pdata playing for
clubs that matched the list in tdata.

You will notice there are new rows created, that is because there may be
more than one player from pdata playing for the club listed from tdata.
The NA correspond to the fact that no player from pdata has a **Club**
matching the list of clubs under tdata.

### inner\_join(x,y)

    inner_join(pdata, tdata) %>% 
    knitr::kable(caption = "Inner join between pdata (x) and tdata (y)")

    ## Joining, by = "Club"

<table>
<caption>Inner join between pdata (x) and tdata (y)</caption>
<thead>
<tr class="header">
<th align="left">First_Name</th>
<th align="left">Last_Name</th>
<th align="left">Club</th>
<th align="left">National_Team</th>
<th align="left">League</th>
<th align="left">League_Country</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Ahmed</td>
<td align="left">Musa</td>
<td align="left">Al-Nassr FC</td>
<td align="left">Nigeria</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Alphonso</td>
<td align="left">Davies</td>
<td align="left">Vancouver Whitecaps</td>
<td align="left">Canada</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Anderson</td>
<td align="left">Talisca</td>
<td align="left">Guangzhou Evergrande</td>
<td align="left">Brazil</td>
<td align="left">Chinese Super League</td>
<td align="left">China</td>
</tr>
<tr class="even">
<td align="left">Andres</td>
<td align="left">Iniesta</td>
<td align="left">FC Barcelona</td>
<td align="left">Spain</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Anice</td>
<td align="left">Badri</td>
<td align="left">Esperance Tunis</td>
<td align="left">Tunisia</td>
<td align="left">Tunisian Ligue Professionnelle 1</td>
<td align="left">Tunisia</td>
</tr>
<tr class="even">
<td align="left">Atsuto</td>
<td align="left">Uchida</td>
<td align="left">Kashima Antlers</td>
<td align="left">Japan</td>
<td align="left">J-League 1</td>
<td align="left">Japan</td>
</tr>
<tr class="odd">
<td align="left">Bafetimbi</td>
<td align="left">Gomis</td>
<td align="left">Al-Hilal FC</td>
<td align="left">France</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Balazs</td>
<td align="left">Dzusudzsak</td>
<td align="left">Al-Ittihad Kalba SC</td>
<td align="left">Hungary</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="odd">
<td align="left">Carlos</td>
<td align="left">Eduardo</td>
<td align="left">Al-Hilal FC</td>
<td align="left">Brazil</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Carlos</td>
<td align="left">Tevez</td>
<td align="left">Boca Juniors</td>
<td align="left">Argentina</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">Cristiano</td>
<td align="left">Ronaldo</td>
<td align="left">Juventus</td>
<td align="left">Portugal</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="even">
<td align="left">Dario</td>
<td align="left">Benedetto</td>
<td align="left">Boca Juniors</td>
<td align="left">Argentina</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">David</td>
<td align="left">de Gea</td>
<td align="left">Manchester United</td>
<td align="left">Spain</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="even">
<td align="left">Diego</td>
<td align="left">Alves</td>
<td align="left">Flamengo</td>
<td align="left">Brazil</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="odd">
<td align="left">Diego</td>
<td align="left">Valeri</td>
<td align="left">Portland Timbers</td>
<td align="left">Argentina</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Diego</td>
<td align="left">Chara</td>
<td align="left">Portland Timbers</td>
<td align="left">Colombia</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Ezequiel</td>
<td align="left">Barco</td>
<td align="left">Atlanta United</td>
<td align="left">Argentina</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Ferjani</td>
<td align="left">Sassi</td>
<td align="left">Zamalek SC</td>
<td align="left">Tunisia</td>
<td align="left">Egyptian Premier League</td>
<td align="left">Egypt</td>
</tr>
<tr class="odd">
<td align="left">Gabriel</td>
<td align="left">Barbosa</td>
<td align="left">Santos FC</td>
<td align="left">Brazil</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Gareth</td>
<td align="left">Bale</td>
<td align="left">Real Madrid</td>
<td align="left">Wales</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Gianluigi</td>
<td align="left">Buffon</td>
<td align="left">Paris St. Germain</td>
<td align="left">Italy</td>
<td align="left">Ligue 1</td>
<td align="left">France</td>
</tr>
<tr class="even">
<td align="left">Giorgio</td>
<td align="left">Chiellini</td>
<td align="left">Juventus</td>
<td align="left">Italy</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="odd">
<td align="left">Jay</td>
<td align="left">Bothroyd</td>
<td align="left">Hokkaido Consadole Sapporo</td>
<td align="left">England</td>
<td align="left">J-League 1</td>
<td align="left">Japan</td>
</tr>
<tr class="even">
<td align="left">Josef</td>
<td align="left">Martinez</td>
<td align="left">Atlanta United</td>
<td align="left">Venezuela</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Karim</td>
<td align="left">Benzema</td>
<td align="left">Real Madrid</td>
<td align="left">France</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="even">
<td align="left">Kei</td>
<td align="left">Kamara</td>
<td align="left">Vancouver Whitecaps</td>
<td align="left">Sierra Leone</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Keisuke</td>
<td align="left">Honda</td>
<td align="left">Melbourne Victory</td>
<td align="left">Japan</td>
<td align="left">A-League</td>
<td align="left">Australia</td>
</tr>
<tr class="even">
<td align="left">Kendall</td>
<td align="left">Waston</td>
<td align="left">Vancouver Whitecaps</td>
<td align="left">Costa Rica</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Kengo</td>
<td align="left">Nakamura</td>
<td align="left">Kawasaki Frontale</td>
<td align="left">Japan</td>
<td align="left">J-League 1</td>
<td align="left">Japan</td>
</tr>
<tr class="even">
<td align="left">Kylian</td>
<td align="left">Mbappe</td>
<td align="left">Paris St. Germain</td>
<td align="left">France</td>
<td align="left">Ligue 1</td>
<td align="left">France</td>
</tr>
<tr class="odd">
<td align="left">Lei</td>
<td align="left">Wu</td>
<td align="left">Shanghai SIPG</td>
<td align="left">China</td>
<td align="left">Chinese Super League</td>
<td align="left">China</td>
</tr>
<tr class="even">
<td align="left">Lionel</td>
<td align="left">Messi</td>
<td align="left">FC Barcelona</td>
<td align="left">Argentina</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Lisandro</td>
<td align="left">Lopez</td>
<td align="left">Racing Club</td>
<td align="left">Argentina</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
</tr>
<tr class="even">
<td align="left">Lucas</td>
<td align="left">Paqueta</td>
<td align="left">Flamengo</td>
<td align="left">Brazil</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="odd">
<td align="left">Lucas</td>
<td align="left">Lima</td>
<td align="left">Palmeiras</td>
<td align="left">Brazil</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Lucho</td>
<td align="left">Gonzalez</td>
<td align="left">Atletico Paranaense</td>
<td align="left">Argentina</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="odd">
<td align="left">Luis</td>
<td align="left">Suarez</td>
<td align="left">FC Barcelona</td>
<td align="left">Uruguay</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="even">
<td align="left">Luka</td>
<td align="left">Modric</td>
<td align="left">Real Madrid</td>
<td align="left">Croatia</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Marcus</td>
<td align="left">Berg</td>
<td align="left">Al-Ain FC</td>
<td align="left">Sweden</td>
<td align="left">Arabian Gulf League</td>
<td align="left">United Arab Emirates</td>
</tr>
<tr class="even">
<td align="left">Marcus</td>
<td align="left">Rashford</td>
<td align="left">Manchester United</td>
<td align="left">England</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="odd">
<td align="left">Miguel</td>
<td align="left">Almiron</td>
<td align="left">Atlanta United</td>
<td align="left">Paraguay</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Miguel</td>
<td align="left">Borja</td>
<td align="left">Palmeiras</td>
<td align="left">Colombia</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="odd">
<td align="left">Milos</td>
<td align="left">Ninkovic</td>
<td align="left">Sydney FC</td>
<td align="left">Serbia</td>
<td align="left">A-League</td>
<td align="left">Australia</td>
</tr>
<tr class="even">
<td align="left">Nana</td>
<td align="left">Kwesi</td>
<td align="left">TP Mazembe</td>
<td align="left">Ghana</td>
<td align="left">Linafoot</td>
<td align="left">Congo</td>
</tr>
<tr class="odd">
<td align="left">Nemanja</td>
<td align="left">Matic</td>
<td align="left">Manchester United</td>
<td align="left">Serbia</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="even">
<td align="left">Neymar</td>
<td align="left">da Silva</td>
<td align="left">Paris St. Germain</td>
<td align="left">Brazil</td>
<td align="left">Ligue 1</td>
<td align="left">France</td>
</tr>
<tr class="odd">
<td align="left">Nordin</td>
<td align="left">Amrabat</td>
<td align="left">Al-Nassr FC</td>
<td align="left">Morocco</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Omar</td>
<td align="left">Al Somah</td>
<td align="left">Al-Ahli Saudi FC</td>
<td align="left">Syria</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="odd">
<td align="left">Oscar</td>
<td align="left">dos Santos</td>
<td align="left">Shanghai SIPG</td>
<td align="left">Brazil</td>
<td align="left">Chinese Super League</td>
<td align="left">China</td>
</tr>
<tr class="even">
<td align="left">Paul</td>
<td align="left">Pogba</td>
<td align="left">Manchester United</td>
<td align="left">France</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="odd">
<td align="left">Paulo</td>
<td align="left">Diaz</td>
<td align="left">Al-Ahli Saudi FC</td>
<td align="left">Chile</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Paulo</td>
<td align="left">Dybala</td>
<td align="left">Juventus</td>
<td align="left">Argentina</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="odd">
<td align="left">Ricardo</td>
<td align="left">Goulart</td>
<td align="left">Guangzhou Evergrande</td>
<td align="left">Brazil</td>
<td align="left">Chinese Super League</td>
<td align="left">China</td>
</tr>
<tr class="even">
<td align="left">Salah</td>
<td align="left">Mohsen</td>
<td align="left">Al-Ahly SC</td>
<td align="left">Egypt</td>
<td align="left">Egyptian Premier League</td>
<td align="left">Egypt</td>
</tr>
<tr class="odd">
<td align="left">Sebastien</td>
<td align="left">Blanco</td>
<td align="left">Portland Timbers</td>
<td align="left">Argentina</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Sergio</td>
<td align="left">Ramos</td>
<td align="left">Real Madrid</td>
<td align="left">Spain</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Siem</td>
<td align="left">De Jong</td>
<td align="left">Sydney FC</td>
<td align="left">Netherlands</td>
<td align="left">A-League</td>
<td align="left">Australia</td>
</tr>
<tr class="even">
<td align="left">Walid</td>
<td align="left">El Karti</td>
<td align="left">Wydad Athletic Club</td>
<td align="left">Morocco</td>
<td align="left">Botola</td>
<td align="left">Morocco</td>
</tr>
<tr class="odd">
<td align="left">Xavi</td>
<td align="left">Hernandez</td>
<td align="left">Al Sadd SC</td>
<td align="left">Spain</td>
<td align="left">Qatar Stars League</td>
<td align="left">Qatar</td>
</tr>
<tr class="even">
<td align="left">Zlatan</td>
<td align="left">Ibrahimovic</td>
<td align="left">LA Galaxy</td>
<td align="left">Sweden</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
</tbody>
</table>

The inner\_join function returns all rows from pdata where there are
matching values in tdata by the variable **Club**, and all columns from
pdata and tdata. Since there are multiple matches by **Club**, as there
are more than one player that plays for the same club under pdata, all
combination of the matches are returned. This is also a mutating join,
and we retained all rows from pdata because there aren't any clubs in
pdata that are missing in tdata.

### semi\_join(x,y)

    semi_join(pdata, tdata) %>% 
    knitr::kable(caption = "Semi join between pdata (x) and tdata (y)")

    ## Joining, by = "Club"

<table>
<caption>Semi join between pdata (x) and tdata (y)</caption>
<thead>
<tr class="header">
<th align="left">First_Name</th>
<th align="left">Last_Name</th>
<th align="left">Club</th>
<th align="left">National_Team</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Ahmed</td>
<td align="left">Musa</td>
<td align="left">Al-Nassr FC</td>
<td align="left">Nigeria</td>
</tr>
<tr class="even">
<td align="left">Alphonso</td>
<td align="left">Davies</td>
<td align="left">Vancouver Whitecaps</td>
<td align="left">Canada</td>
</tr>
<tr class="odd">
<td align="left">Anderson</td>
<td align="left">Talisca</td>
<td align="left">Guangzhou Evergrande</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Andres</td>
<td align="left">Iniesta</td>
<td align="left">FC Barcelona</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Anice</td>
<td align="left">Badri</td>
<td align="left">Esperance Tunis</td>
<td align="left">Tunisia</td>
</tr>
<tr class="even">
<td align="left">Atsuto</td>
<td align="left">Uchida</td>
<td align="left">Kashima Antlers</td>
<td align="left">Japan</td>
</tr>
<tr class="odd">
<td align="left">Bafetimbi</td>
<td align="left">Gomis</td>
<td align="left">Al-Hilal FC</td>
<td align="left">France</td>
</tr>
<tr class="even">
<td align="left">Balazs</td>
<td align="left">Dzusudzsak</td>
<td align="left">Al-Ittihad Kalba SC</td>
<td align="left">Hungary</td>
</tr>
<tr class="odd">
<td align="left">Carlos</td>
<td align="left">Eduardo</td>
<td align="left">Al-Hilal FC</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Carlos</td>
<td align="left">Tevez</td>
<td align="left">Boca Juniors</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">Cristiano</td>
<td align="left">Ronaldo</td>
<td align="left">Juventus</td>
<td align="left">Portugal</td>
</tr>
<tr class="even">
<td align="left">Dario</td>
<td align="left">Benedetto</td>
<td align="left">Boca Juniors</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">David</td>
<td align="left">de Gea</td>
<td align="left">Manchester United</td>
<td align="left">Spain</td>
</tr>
<tr class="even">
<td align="left">Diego</td>
<td align="left">Alves</td>
<td align="left">Flamengo</td>
<td align="left">Brazil</td>
</tr>
<tr class="odd">
<td align="left">Diego</td>
<td align="left">Valeri</td>
<td align="left">Portland Timbers</td>
<td align="left">Argentina</td>
</tr>
<tr class="even">
<td align="left">Diego</td>
<td align="left">Chara</td>
<td align="left">Portland Timbers</td>
<td align="left">Colombia</td>
</tr>
<tr class="odd">
<td align="left">Ezequiel</td>
<td align="left">Barco</td>
<td align="left">Atlanta United</td>
<td align="left">Argentina</td>
</tr>
<tr class="even">
<td align="left">Ferjani</td>
<td align="left">Sassi</td>
<td align="left">Zamalek SC</td>
<td align="left">Tunisia</td>
</tr>
<tr class="odd">
<td align="left">Gabriel</td>
<td align="left">Barbosa</td>
<td align="left">Santos FC</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Gareth</td>
<td align="left">Bale</td>
<td align="left">Real Madrid</td>
<td align="left">Wales</td>
</tr>
<tr class="odd">
<td align="left">Gianluigi</td>
<td align="left">Buffon</td>
<td align="left">Paris St. Germain</td>
<td align="left">Italy</td>
</tr>
<tr class="even">
<td align="left">Giorgio</td>
<td align="left">Chiellini</td>
<td align="left">Juventus</td>
<td align="left">Italy</td>
</tr>
<tr class="odd">
<td align="left">Jay</td>
<td align="left">Bothroyd</td>
<td align="left">Hokkaido Consadole Sapporo</td>
<td align="left">England</td>
</tr>
<tr class="even">
<td align="left">Josef</td>
<td align="left">Martinez</td>
<td align="left">Atlanta United</td>
<td align="left">Venezuela</td>
</tr>
<tr class="odd">
<td align="left">Karim</td>
<td align="left">Benzema</td>
<td align="left">Real Madrid</td>
<td align="left">France</td>
</tr>
<tr class="even">
<td align="left">Kei</td>
<td align="left">Kamara</td>
<td align="left">Vancouver Whitecaps</td>
<td align="left">Sierra Leone</td>
</tr>
<tr class="odd">
<td align="left">Keisuke</td>
<td align="left">Honda</td>
<td align="left">Melbourne Victory</td>
<td align="left">Japan</td>
</tr>
<tr class="even">
<td align="left">Kendall</td>
<td align="left">Waston</td>
<td align="left">Vancouver Whitecaps</td>
<td align="left">Costa Rica</td>
</tr>
<tr class="odd">
<td align="left">Kengo</td>
<td align="left">Nakamura</td>
<td align="left">Kawasaki Frontale</td>
<td align="left">Japan</td>
</tr>
<tr class="even">
<td align="left">Kylian</td>
<td align="left">Mbappe</td>
<td align="left">Paris St. Germain</td>
<td align="left">France</td>
</tr>
<tr class="odd">
<td align="left">Lei</td>
<td align="left">Wu</td>
<td align="left">Shanghai SIPG</td>
<td align="left">China</td>
</tr>
<tr class="even">
<td align="left">Lionel</td>
<td align="left">Messi</td>
<td align="left">FC Barcelona</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">Lisandro</td>
<td align="left">Lopez</td>
<td align="left">Racing Club</td>
<td align="left">Argentina</td>
</tr>
<tr class="even">
<td align="left">Lucas</td>
<td align="left">Paqueta</td>
<td align="left">Flamengo</td>
<td align="left">Brazil</td>
</tr>
<tr class="odd">
<td align="left">Lucas</td>
<td align="left">Lima</td>
<td align="left">Palmeiras</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Lucho</td>
<td align="left">Gonzalez</td>
<td align="left">Atletico Paranaense</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">Luis</td>
<td align="left">Suarez</td>
<td align="left">FC Barcelona</td>
<td align="left">Uruguay</td>
</tr>
<tr class="even">
<td align="left">Luka</td>
<td align="left">Modric</td>
<td align="left">Real Madrid</td>
<td align="left">Croatia</td>
</tr>
<tr class="odd">
<td align="left">Marcus</td>
<td align="left">Berg</td>
<td align="left">Al-Ain FC</td>
<td align="left">Sweden</td>
</tr>
<tr class="even">
<td align="left">Marcus</td>
<td align="left">Rashford</td>
<td align="left">Manchester United</td>
<td align="left">England</td>
</tr>
<tr class="odd">
<td align="left">Miguel</td>
<td align="left">Almiron</td>
<td align="left">Atlanta United</td>
<td align="left">Paraguay</td>
</tr>
<tr class="even">
<td align="left">Miguel</td>
<td align="left">Borja</td>
<td align="left">Palmeiras</td>
<td align="left">Colombia</td>
</tr>
<tr class="odd">
<td align="left">Milos</td>
<td align="left">Ninkovic</td>
<td align="left">Sydney FC</td>
<td align="left">Serbia</td>
</tr>
<tr class="even">
<td align="left">Nana</td>
<td align="left">Kwesi</td>
<td align="left">TP Mazembe</td>
<td align="left">Ghana</td>
</tr>
<tr class="odd">
<td align="left">Nemanja</td>
<td align="left">Matic</td>
<td align="left">Manchester United</td>
<td align="left">Serbia</td>
</tr>
<tr class="even">
<td align="left">Neymar</td>
<td align="left">da Silva</td>
<td align="left">Paris St. Germain</td>
<td align="left">Brazil</td>
</tr>
<tr class="odd">
<td align="left">Nordin</td>
<td align="left">Amrabat</td>
<td align="left">Al-Nassr FC</td>
<td align="left">Morocco</td>
</tr>
<tr class="even">
<td align="left">Omar</td>
<td align="left">Al Somah</td>
<td align="left">Al-Ahli Saudi FC</td>
<td align="left">Syria</td>
</tr>
<tr class="odd">
<td align="left">Oscar</td>
<td align="left">dos Santos</td>
<td align="left">Shanghai SIPG</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Paul</td>
<td align="left">Pogba</td>
<td align="left">Manchester United</td>
<td align="left">France</td>
</tr>
<tr class="odd">
<td align="left">Paulo</td>
<td align="left">Diaz</td>
<td align="left">Al-Ahli Saudi FC</td>
<td align="left">Chile</td>
</tr>
<tr class="even">
<td align="left">Paulo</td>
<td align="left">Dybala</td>
<td align="left">Juventus</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">Ricardo</td>
<td align="left">Goulart</td>
<td align="left">Guangzhou Evergrande</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Salah</td>
<td align="left">Mohsen</td>
<td align="left">Al-Ahly SC</td>
<td align="left">Egypt</td>
</tr>
<tr class="odd">
<td align="left">Sebastien</td>
<td align="left">Blanco</td>
<td align="left">Portland Timbers</td>
<td align="left">Argentina</td>
</tr>
<tr class="even">
<td align="left">Sergio</td>
<td align="left">Ramos</td>
<td align="left">Real Madrid</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Siem</td>
<td align="left">De Jong</td>
<td align="left">Sydney FC</td>
<td align="left">Netherlands</td>
</tr>
<tr class="even">
<td align="left">Walid</td>
<td align="left">El Karti</td>
<td align="left">Wydad Athletic Club</td>
<td align="left">Morocco</td>
</tr>
<tr class="odd">
<td align="left">Xavi</td>
<td align="left">Hernandez</td>
<td align="left">Al Sadd SC</td>
<td align="left">Spain</td>
</tr>
<tr class="even">
<td align="left">Zlatan</td>
<td align="left">Ibrahimovic</td>
<td align="left">LA Galaxy</td>
<td align="left">Sweden</td>
</tr>
</tbody>
</table>

The semi\_join function returns all rows from pdata where there are
matching values in **Club** with tdata, but the semi\_join keeps just
the columns from x. Once again, all data is retained because all the
entries in **Club** from pdata also exist in tdata.

### full\_join(x,y)

    full_join(pdata, tdata) %>% 
    knitr::kable(caption = "Full join between pdata (x) and tdata (y)")

    ## Joining, by = "Club"

<table>
<caption>Full join between pdata (x) and tdata (y)</caption>
<thead>
<tr class="header">
<th align="left">First_Name</th>
<th align="left">Last_Name</th>
<th align="left">Club</th>
<th align="left">National_Team</th>
<th align="left">League</th>
<th align="left">League_Country</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Ahmed</td>
<td align="left">Musa</td>
<td align="left">Al-Nassr FC</td>
<td align="left">Nigeria</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Alphonso</td>
<td align="left">Davies</td>
<td align="left">Vancouver Whitecaps</td>
<td align="left">Canada</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Anderson</td>
<td align="left">Talisca</td>
<td align="left">Guangzhou Evergrande</td>
<td align="left">Brazil</td>
<td align="left">Chinese Super League</td>
<td align="left">China</td>
</tr>
<tr class="even">
<td align="left">Andres</td>
<td align="left">Iniesta</td>
<td align="left">FC Barcelona</td>
<td align="left">Spain</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Anice</td>
<td align="left">Badri</td>
<td align="left">Esperance Tunis</td>
<td align="left">Tunisia</td>
<td align="left">Tunisian Ligue Professionnelle 1</td>
<td align="left">Tunisia</td>
</tr>
<tr class="even">
<td align="left">Atsuto</td>
<td align="left">Uchida</td>
<td align="left">Kashima Antlers</td>
<td align="left">Japan</td>
<td align="left">J-League 1</td>
<td align="left">Japan</td>
</tr>
<tr class="odd">
<td align="left">Bafetimbi</td>
<td align="left">Gomis</td>
<td align="left">Al-Hilal FC</td>
<td align="left">France</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Balazs</td>
<td align="left">Dzusudzsak</td>
<td align="left">Al-Ittihad Kalba SC</td>
<td align="left">Hungary</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="odd">
<td align="left">Carlos</td>
<td align="left">Eduardo</td>
<td align="left">Al-Hilal FC</td>
<td align="left">Brazil</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Carlos</td>
<td align="left">Tevez</td>
<td align="left">Boca Juniors</td>
<td align="left">Argentina</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">Cristiano</td>
<td align="left">Ronaldo</td>
<td align="left">Juventus</td>
<td align="left">Portugal</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="even">
<td align="left">Dario</td>
<td align="left">Benedetto</td>
<td align="left">Boca Juniors</td>
<td align="left">Argentina</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">David</td>
<td align="left">de Gea</td>
<td align="left">Manchester United</td>
<td align="left">Spain</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="even">
<td align="left">Diego</td>
<td align="left">Alves</td>
<td align="left">Flamengo</td>
<td align="left">Brazil</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="odd">
<td align="left">Diego</td>
<td align="left">Valeri</td>
<td align="left">Portland Timbers</td>
<td align="left">Argentina</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Diego</td>
<td align="left">Chara</td>
<td align="left">Portland Timbers</td>
<td align="left">Colombia</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Ezequiel</td>
<td align="left">Barco</td>
<td align="left">Atlanta United</td>
<td align="left">Argentina</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Ferjani</td>
<td align="left">Sassi</td>
<td align="left">Zamalek SC</td>
<td align="left">Tunisia</td>
<td align="left">Egyptian Premier League</td>
<td align="left">Egypt</td>
</tr>
<tr class="odd">
<td align="left">Gabriel</td>
<td align="left">Barbosa</td>
<td align="left">Santos FC</td>
<td align="left">Brazil</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Gareth</td>
<td align="left">Bale</td>
<td align="left">Real Madrid</td>
<td align="left">Wales</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Gianluigi</td>
<td align="left">Buffon</td>
<td align="left">Paris St. Germain</td>
<td align="left">Italy</td>
<td align="left">Ligue 1</td>
<td align="left">France</td>
</tr>
<tr class="even">
<td align="left">Giorgio</td>
<td align="left">Chiellini</td>
<td align="left">Juventus</td>
<td align="left">Italy</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="odd">
<td align="left">Jay</td>
<td align="left">Bothroyd</td>
<td align="left">Hokkaido Consadole Sapporo</td>
<td align="left">England</td>
<td align="left">J-League 1</td>
<td align="left">Japan</td>
</tr>
<tr class="even">
<td align="left">Josef</td>
<td align="left">Martinez</td>
<td align="left">Atlanta United</td>
<td align="left">Venezuela</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Karim</td>
<td align="left">Benzema</td>
<td align="left">Real Madrid</td>
<td align="left">France</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="even">
<td align="left">Kei</td>
<td align="left">Kamara</td>
<td align="left">Vancouver Whitecaps</td>
<td align="left">Sierra Leone</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Keisuke</td>
<td align="left">Honda</td>
<td align="left">Melbourne Victory</td>
<td align="left">Japan</td>
<td align="left">A-League</td>
<td align="left">Australia</td>
</tr>
<tr class="even">
<td align="left">Kendall</td>
<td align="left">Waston</td>
<td align="left">Vancouver Whitecaps</td>
<td align="left">Costa Rica</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Kengo</td>
<td align="left">Nakamura</td>
<td align="left">Kawasaki Frontale</td>
<td align="left">Japan</td>
<td align="left">J-League 1</td>
<td align="left">Japan</td>
</tr>
<tr class="even">
<td align="left">Kylian</td>
<td align="left">Mbappe</td>
<td align="left">Paris St. Germain</td>
<td align="left">France</td>
<td align="left">Ligue 1</td>
<td align="left">France</td>
</tr>
<tr class="odd">
<td align="left">Lei</td>
<td align="left">Wu</td>
<td align="left">Shanghai SIPG</td>
<td align="left">China</td>
<td align="left">Chinese Super League</td>
<td align="left">China</td>
</tr>
<tr class="even">
<td align="left">Lionel</td>
<td align="left">Messi</td>
<td align="left">FC Barcelona</td>
<td align="left">Argentina</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Lisandro</td>
<td align="left">Lopez</td>
<td align="left">Racing Club</td>
<td align="left">Argentina</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
</tr>
<tr class="even">
<td align="left">Lucas</td>
<td align="left">Paqueta</td>
<td align="left">Flamengo</td>
<td align="left">Brazil</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="odd">
<td align="left">Lucas</td>
<td align="left">Lima</td>
<td align="left">Palmeiras</td>
<td align="left">Brazil</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Lucho</td>
<td align="left">Gonzalez</td>
<td align="left">Atletico Paranaense</td>
<td align="left">Argentina</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="odd">
<td align="left">Luis</td>
<td align="left">Suarez</td>
<td align="left">FC Barcelona</td>
<td align="left">Uruguay</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="even">
<td align="left">Luka</td>
<td align="left">Modric</td>
<td align="left">Real Madrid</td>
<td align="left">Croatia</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Marcus</td>
<td align="left">Berg</td>
<td align="left">Al-Ain FC</td>
<td align="left">Sweden</td>
<td align="left">Arabian Gulf League</td>
<td align="left">United Arab Emirates</td>
</tr>
<tr class="even">
<td align="left">Marcus</td>
<td align="left">Rashford</td>
<td align="left">Manchester United</td>
<td align="left">England</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="odd">
<td align="left">Miguel</td>
<td align="left">Almiron</td>
<td align="left">Atlanta United</td>
<td align="left">Paraguay</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Miguel</td>
<td align="left">Borja</td>
<td align="left">Palmeiras</td>
<td align="left">Colombia</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="odd">
<td align="left">Milos</td>
<td align="left">Ninkovic</td>
<td align="left">Sydney FC</td>
<td align="left">Serbia</td>
<td align="left">A-League</td>
<td align="left">Australia</td>
</tr>
<tr class="even">
<td align="left">Nana</td>
<td align="left">Kwesi</td>
<td align="left">TP Mazembe</td>
<td align="left">Ghana</td>
<td align="left">Linafoot</td>
<td align="left">Congo</td>
</tr>
<tr class="odd">
<td align="left">Nemanja</td>
<td align="left">Matic</td>
<td align="left">Manchester United</td>
<td align="left">Serbia</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="even">
<td align="left">Neymar</td>
<td align="left">da Silva</td>
<td align="left">Paris St. Germain</td>
<td align="left">Brazil</td>
<td align="left">Ligue 1</td>
<td align="left">France</td>
</tr>
<tr class="odd">
<td align="left">Nordin</td>
<td align="left">Amrabat</td>
<td align="left">Al-Nassr FC</td>
<td align="left">Morocco</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Omar</td>
<td align="left">Al Somah</td>
<td align="left">Al-Ahli Saudi FC</td>
<td align="left">Syria</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="odd">
<td align="left">Oscar</td>
<td align="left">dos Santos</td>
<td align="left">Shanghai SIPG</td>
<td align="left">Brazil</td>
<td align="left">Chinese Super League</td>
<td align="left">China</td>
</tr>
<tr class="even">
<td align="left">Paul</td>
<td align="left">Pogba</td>
<td align="left">Manchester United</td>
<td align="left">France</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="odd">
<td align="left">Paulo</td>
<td align="left">Diaz</td>
<td align="left">Al-Ahli Saudi FC</td>
<td align="left">Chile</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Paulo</td>
<td align="left">Dybala</td>
<td align="left">Juventus</td>
<td align="left">Argentina</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="odd">
<td align="left">Ricardo</td>
<td align="left">Goulart</td>
<td align="left">Guangzhou Evergrande</td>
<td align="left">Brazil</td>
<td align="left">Chinese Super League</td>
<td align="left">China</td>
</tr>
<tr class="even">
<td align="left">Salah</td>
<td align="left">Mohsen</td>
<td align="left">Al-Ahly SC</td>
<td align="left">Egypt</td>
<td align="left">Egyptian Premier League</td>
<td align="left">Egypt</td>
</tr>
<tr class="odd">
<td align="left">Sebastien</td>
<td align="left">Blanco</td>
<td align="left">Portland Timbers</td>
<td align="left">Argentina</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Sergio</td>
<td align="left">Ramos</td>
<td align="left">Real Madrid</td>
<td align="left">Spain</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Siem</td>
<td align="left">De Jong</td>
<td align="left">Sydney FC</td>
<td align="left">Netherlands</td>
<td align="left">A-League</td>
<td align="left">Australia</td>
</tr>
<tr class="even">
<td align="left">Walid</td>
<td align="left">El Karti</td>
<td align="left">Wydad Athletic Club</td>
<td align="left">Morocco</td>
<td align="left">Botola</td>
<td align="left">Morocco</td>
</tr>
<tr class="odd">
<td align="left">Xavi</td>
<td align="left">Hernandez</td>
<td align="left">Al Sadd SC</td>
<td align="left">Spain</td>
<td align="left">Qatar Stars League</td>
<td align="left">Qatar</td>
</tr>
<tr class="even">
<td align="left">Zlatan</td>
<td align="left">Ibrahimovic</td>
<td align="left">LA Galaxy</td>
<td align="left">Sweden</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">AC Milan</td>
<td align="left">NA</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="even">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Ajax Amsterdam</td>
<td align="left">NA</td>
<td align="left">Eredivisie</td>
<td align="left">Netherlands</td>
</tr>
<tr class="odd">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Arsenal FC</td>
<td align="left">NA</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="even">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Bate Borisov</td>
<td align="left">NA</td>
<td align="left">Belarusian Premier League</td>
<td align="left">Belarus</td>
</tr>
<tr class="odd">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Bayern Munich</td>
<td align="left">NA</td>
<td align="left">Bundesliga</td>
<td align="left">Germany</td>
</tr>
<tr class="even">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Club Brugge</td>
<td align="left">NA</td>
<td align="left">Belgian Pro League</td>
<td align="left">Belgium</td>
</tr>
<tr class="odd">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">CSKA Moscow</td>
<td align="left">NA</td>
<td align="left">Russian Premier League</td>
<td align="left">Russain</td>
</tr>
<tr class="even">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Feyenoord</td>
<td align="left">NA</td>
<td align="left">Eredivisie</td>
<td align="left">Netherlands</td>
</tr>
<tr class="odd">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Inter Milan</td>
<td align="left">NA</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="even">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Manchester City</td>
<td align="left">NA</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="odd">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Napoli SSC</td>
<td align="left">NA</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="even">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Olympique Lyonnais</td>
<td align="left">NA</td>
<td align="left">Ligue 1</td>
<td align="left">France</td>
</tr>
<tr class="odd">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Olympique Marseille</td>
<td align="left">NA</td>
<td align="left">Ligue 1</td>
<td align="left">France</td>
</tr>
<tr class="even">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Schalke 04</td>
<td align="left">NA</td>
<td align="left">Bundesliga</td>
<td align="left">Germany</td>
</tr>
<tr class="odd">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Seattle Sounders</td>
<td align="left">NA</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Sevilla</td>
<td align="left">NA</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">Valencia CF</td>
<td align="left">NA</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
</tbody>
</table>

The full\_join function return all possible combinations (all rows and
all columns from both pdata and tdata). When there are no matching
values for **Club**, the table returns NA for whichever variable has
missing entries.

### anti\_join(x,y)

    anti_join(pdata, tdata) # no table generated

    ## Joining, by = "Club"

    ## # A tibble: 0 x 4
    ## # ... with 4 variables: First_Name <chr>, Last_Name <chr>, Club <chr>,
    ## #   National_Team <chr>

Anti\_join returns all rows from pdata where there are **non-matching**
values for **Club** in tdata, and keeps only just the columns from
pdata. This output returns no rows, because all unique values in
**Club** from pdata matches the values of **Club** from tdata.

### Changing Arguments for Join Functions

It would be interesting to see how the join functions work, if we flip
tdata to be our first argument and pdata as our second. The following
set of join functions are a repeat of what I just went through, with the
exception of the changed ordering of the datasets in the argument for
each join function.

To spare you the details for each table, I will include the first 20
entries of the new table from the join functions

### left\_join(y,x)

    lj<- left_join(tdata, pdata) 

    ## Joining, by = "Club"

    knitr::kable(lj[1:20,], caption = "Left join between tdata (x) and pdata (y)")

<table>
<caption>Left join between tdata (x) and pdata (y)</caption>
<thead>
<tr class="header">
<th align="left">Club</th>
<th align="left">League</th>
<th align="left">League_Country</th>
<th align="left">First_Name</th>
<th align="left">Last_Name</th>
<th align="left">National_Team</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">AC Milan</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">NA</td>
</tr>
<tr class="even">
<td align="left">Ajax Amsterdam</td>
<td align="left">Eredivisie</td>
<td align="left">Netherlands</td>
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">NA</td>
</tr>
<tr class="odd">
<td align="left">Al Sadd SC</td>
<td align="left">Qatar Stars League</td>
<td align="left">Qatar</td>
<td align="left">Xavi</td>
<td align="left">Hernandez</td>
<td align="left">Spain</td>
</tr>
<tr class="even">
<td align="left">Al-Ahli Saudi FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Omar</td>
<td align="left">Al Somah</td>
<td align="left">Syria</td>
</tr>
<tr class="odd">
<td align="left">Al-Ahli Saudi FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Paulo</td>
<td align="left">Diaz</td>
<td align="left">Chile</td>
</tr>
<tr class="even">
<td align="left">Al-Ahly SC</td>
<td align="left">Egyptian Premier League</td>
<td align="left">Egypt</td>
<td align="left">Salah</td>
<td align="left">Mohsen</td>
<td align="left">Egypt</td>
</tr>
<tr class="odd">
<td align="left">Al-Ain FC</td>
<td align="left">Arabian Gulf League</td>
<td align="left">United Arab Emirates</td>
<td align="left">Marcus</td>
<td align="left">Berg</td>
<td align="left">Sweden</td>
</tr>
<tr class="even">
<td align="left">Al-Hilal FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Bafetimbi</td>
<td align="left">Gomis</td>
<td align="left">France</td>
</tr>
<tr class="odd">
<td align="left">Al-Hilal FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Carlos</td>
<td align="left">Eduardo</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Al-Ittihad Kalba SC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Balazs</td>
<td align="left">Dzusudzsak</td>
<td align="left">Hungary</td>
</tr>
<tr class="odd">
<td align="left">Al-Nassr FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Ahmed</td>
<td align="left">Musa</td>
<td align="left">Nigeria</td>
</tr>
<tr class="even">
<td align="left">Al-Nassr FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Nordin</td>
<td align="left">Amrabat</td>
<td align="left">Morocco</td>
</tr>
<tr class="odd">
<td align="left">Arsenal FC</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">NA</td>
</tr>
<tr class="even">
<td align="left">Atlanta United</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
<td align="left">Ezequiel</td>
<td align="left">Barco</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">Atlanta United</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
<td align="left">Josef</td>
<td align="left">Martinez</td>
<td align="left">Venezuela</td>
</tr>
<tr class="even">
<td align="left">Atlanta United</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
<td align="left">Miguel</td>
<td align="left">Almiron</td>
<td align="left">Paraguay</td>
</tr>
<tr class="odd">
<td align="left">Atletico Paranaense</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
<td align="left">Lucho</td>
<td align="left">Gonzalez</td>
<td align="left">Argentina</td>
</tr>
<tr class="even">
<td align="left">Bate Borisov</td>
<td align="left">Belarusian Premier League</td>
<td align="left">Belarus</td>
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">NA</td>
</tr>
<tr class="odd">
<td align="left">Bayern Munich</td>
<td align="left">Bundesliga</td>
<td align="left">Germany</td>
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">NA</td>
</tr>
<tr class="even">
<td align="left">Boca Juniors</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
<td align="left">Carlos</td>
<td align="left">Tevez</td>
<td align="left">Argentina</td>
</tr>
</tbody>
</table>

The **left\_join** function is a mutating join that returns all rows
from tdata and adds columns from pdata. This function matches tdata and
odata by the variable **Club**. If **Club** from tdata and pdata match
then the function will bind additional information from pdata such as
**First\_Name**, **Last\_Name**, and **National\_Team** to tdata. For
**Club** not matched between tdata and pdata, there will be NA's from
the new columns binded from pdata

### right\_join(y,x)

    rj <- right_join(tdata, pdata) 

    ## Joining, by = "Club"

    knitr::kable(rj[1:20,], caption = "Right join between tdata (x) and odata (y)")

<table>
<caption>Right join between tdata (x) and odata (y)</caption>
<thead>
<tr class="header">
<th align="left">Club</th>
<th align="left">League</th>
<th align="left">League_Country</th>
<th align="left">First_Name</th>
<th align="left">Last_Name</th>
<th align="left">National_Team</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Al-Nassr FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Ahmed</td>
<td align="left">Musa</td>
<td align="left">Nigeria</td>
</tr>
<tr class="even">
<td align="left">Vancouver Whitecaps</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
<td align="left">Alphonso</td>
<td align="left">Davies</td>
<td align="left">Canada</td>
</tr>
<tr class="odd">
<td align="left">Guangzhou Evergrande</td>
<td align="left">Chinese Super League</td>
<td align="left">China</td>
<td align="left">Anderson</td>
<td align="left">Talisca</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">FC Barcelona</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
<td align="left">Andres</td>
<td align="left">Iniesta</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Esperance Tunis</td>
<td align="left">Tunisian Ligue Professionnelle 1</td>
<td align="left">Tunisia</td>
<td align="left">Anice</td>
<td align="left">Badri</td>
<td align="left">Tunisia</td>
</tr>
<tr class="even">
<td align="left">Kashima Antlers</td>
<td align="left">J-League 1</td>
<td align="left">Japan</td>
<td align="left">Atsuto</td>
<td align="left">Uchida</td>
<td align="left">Japan</td>
</tr>
<tr class="odd">
<td align="left">Al-Hilal FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Bafetimbi</td>
<td align="left">Gomis</td>
<td align="left">France</td>
</tr>
<tr class="even">
<td align="left">Al-Ittihad Kalba SC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Balazs</td>
<td align="left">Dzusudzsak</td>
<td align="left">Hungary</td>
</tr>
<tr class="odd">
<td align="left">Al-Hilal FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Carlos</td>
<td align="left">Eduardo</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Boca Juniors</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
<td align="left">Carlos</td>
<td align="left">Tevez</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">Juventus</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
<td align="left">Cristiano</td>
<td align="left">Ronaldo</td>
<td align="left">Portugal</td>
</tr>
<tr class="even">
<td align="left">Boca Juniors</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
<td align="left">Dario</td>
<td align="left">Benedetto</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">Manchester United</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
<td align="left">David</td>
<td align="left">de Gea</td>
<td align="left">Spain</td>
</tr>
<tr class="even">
<td align="left">Flamengo</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
<td align="left">Diego</td>
<td align="left">Alves</td>
<td align="left">Brazil</td>
</tr>
<tr class="odd">
<td align="left">Portland Timbers</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
<td align="left">Diego</td>
<td align="left">Valeri</td>
<td align="left">Argentina</td>
</tr>
<tr class="even">
<td align="left">Portland Timbers</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
<td align="left">Diego</td>
<td align="left">Chara</td>
<td align="left">Colombia</td>
</tr>
<tr class="odd">
<td align="left">Atlanta United</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
<td align="left">Ezequiel</td>
<td align="left">Barco</td>
<td align="left">Argentina</td>
</tr>
<tr class="even">
<td align="left">Zamalek SC</td>
<td align="left">Egyptian Premier League</td>
<td align="left">Egypt</td>
<td align="left">Ferjani</td>
<td align="left">Sassi</td>
<td align="left">Tunisia</td>
</tr>
<tr class="odd">
<td align="left">Santos FC</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
<td align="left">Gabriel</td>
<td align="left">Barbosa</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Real Madrid</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
<td align="left">Gareth</td>
<td align="left">Bale</td>
<td align="left">Wales</td>
</tr>
</tbody>
</table>

The **right\_join** function is a mutating join that returns all rows
from 'x' (tdata) and adds columns from 'y' (pdata). This function
matches tdata and pdata by the variable **Club**. If **Club** from tdata
and pdata match then the function will bind additional information from
pdata such as **First\_Name**, **Last\_Name**,and **Nationality** to
tdata. The new table tells us all player names, and adds the team
information from tdata, for clubs that matched the list in pdata.

### inner\_join(y,x)

    ij <- inner_join(tdata, pdata)

    ## Joining, by = "Club"

    knitr::kable(ij[1:20,], caption = "Inner join between tdata (x) and odata (y)")

<table>
<caption>Inner join between tdata (x) and odata (y)</caption>
<thead>
<tr class="header">
<th align="left">Club</th>
<th align="left">League</th>
<th align="left">League_Country</th>
<th align="left">First_Name</th>
<th align="left">Last_Name</th>
<th align="left">National_Team</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Al Sadd SC</td>
<td align="left">Qatar Stars League</td>
<td align="left">Qatar</td>
<td align="left">Xavi</td>
<td align="left">Hernandez</td>
<td align="left">Spain</td>
</tr>
<tr class="even">
<td align="left">Al-Ahli Saudi FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Omar</td>
<td align="left">Al Somah</td>
<td align="left">Syria</td>
</tr>
<tr class="odd">
<td align="left">Al-Ahli Saudi FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Paulo</td>
<td align="left">Diaz</td>
<td align="left">Chile</td>
</tr>
<tr class="even">
<td align="left">Al-Ahly SC</td>
<td align="left">Egyptian Premier League</td>
<td align="left">Egypt</td>
<td align="left">Salah</td>
<td align="left">Mohsen</td>
<td align="left">Egypt</td>
</tr>
<tr class="odd">
<td align="left">Al-Ain FC</td>
<td align="left">Arabian Gulf League</td>
<td align="left">United Arab Emirates</td>
<td align="left">Marcus</td>
<td align="left">Berg</td>
<td align="left">Sweden</td>
</tr>
<tr class="even">
<td align="left">Al-Hilal FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Bafetimbi</td>
<td align="left">Gomis</td>
<td align="left">France</td>
</tr>
<tr class="odd">
<td align="left">Al-Hilal FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Carlos</td>
<td align="left">Eduardo</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Al-Ittihad Kalba SC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Balazs</td>
<td align="left">Dzusudzsak</td>
<td align="left">Hungary</td>
</tr>
<tr class="odd">
<td align="left">Al-Nassr FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Ahmed</td>
<td align="left">Musa</td>
<td align="left">Nigeria</td>
</tr>
<tr class="even">
<td align="left">Al-Nassr FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Nordin</td>
<td align="left">Amrabat</td>
<td align="left">Morocco</td>
</tr>
<tr class="odd">
<td align="left">Atlanta United</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
<td align="left">Ezequiel</td>
<td align="left">Barco</td>
<td align="left">Argentina</td>
</tr>
<tr class="even">
<td align="left">Atlanta United</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
<td align="left">Josef</td>
<td align="left">Martinez</td>
<td align="left">Venezuela</td>
</tr>
<tr class="odd">
<td align="left">Atlanta United</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
<td align="left">Miguel</td>
<td align="left">Almiron</td>
<td align="left">Paraguay</td>
</tr>
<tr class="even">
<td align="left">Atletico Paranaense</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
<td align="left">Lucho</td>
<td align="left">Gonzalez</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">Boca Juniors</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
<td align="left">Carlos</td>
<td align="left">Tevez</td>
<td align="left">Argentina</td>
</tr>
<tr class="even">
<td align="left">Boca Juniors</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
<td align="left">Dario</td>
<td align="left">Benedetto</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">Esperance Tunis</td>
<td align="left">Tunisian Ligue Professionnelle 1</td>
<td align="left">Tunisia</td>
<td align="left">Anice</td>
<td align="left">Badri</td>
<td align="left">Tunisia</td>
</tr>
<tr class="even">
<td align="left">FC Barcelona</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
<td align="left">Andres</td>
<td align="left">Iniesta</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">FC Barcelona</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
<td align="left">Lionel</td>
<td align="left">Messi</td>
<td align="left">Argentina</td>
</tr>
<tr class="even">
<td align="left">FC Barcelona</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
<td align="left">Luis</td>
<td align="left">Suarez</td>
<td align="left">Uruguay</td>
</tr>
</tbody>
</table>

The inner\_join function returns all rows from tdata where there are
matching values in pdata by the variable **Club**, and all columns from
pdata and tdata. Since there are multiple matches by **Club**, as there
are more than one player that plays for the same club under pdata, all
combination of the matches are returned. This is also a mutating join,
but the exception here is that we did not retain the rows from tdata for
clubs that did not match the list in pdata.

### semi\_join(y,x)

    sj <- semi_join(tdata, pdata)

    ## Joining, by = "Club"

    knitr::kable(sj[1:20,], caption = "Semi join between tdata (x) and pdata (y)")

<table>
<caption>Semi join between tdata (x) and pdata (y)</caption>
<thead>
<tr class="header">
<th align="left">Club</th>
<th align="left">League</th>
<th align="left">League_Country</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Al Sadd SC</td>
<td align="left">Qatar Stars League</td>
<td align="left">Qatar</td>
</tr>
<tr class="even">
<td align="left">Al-Ahli Saudi FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="odd">
<td align="left">Al-Ahly SC</td>
<td align="left">Egyptian Premier League</td>
<td align="left">Egypt</td>
</tr>
<tr class="even">
<td align="left">Al-Ain FC</td>
<td align="left">Arabian Gulf League</td>
<td align="left">United Arab Emirates</td>
</tr>
<tr class="odd">
<td align="left">Al-Hilal FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Al-Ittihad Kalba SC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="odd">
<td align="left">Al-Nassr FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
</tr>
<tr class="even">
<td align="left">Atlanta United</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="odd">
<td align="left">Atletico Paranaense</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Boca Juniors</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">Esperance Tunis</td>
<td align="left">Tunisian Ligue Professionnelle 1</td>
<td align="left">Tunisia</td>
</tr>
<tr class="even">
<td align="left">FC Barcelona</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Flamengo</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Guangzhou Evergrande</td>
<td align="left">Chinese Super League</td>
<td align="left">China</td>
</tr>
<tr class="odd">
<td align="left">Hokkaido Consadole Sapporo</td>
<td align="left">J-League 1</td>
<td align="left">Japan</td>
</tr>
<tr class="even">
<td align="left">Juventus</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="odd">
<td align="left">Kashima Antlers</td>
<td align="left">J-League 1</td>
<td align="left">Japan</td>
</tr>
<tr class="even">
<td align="left">Kawasaki Frontale</td>
<td align="left">J-League 1</td>
<td align="left">Japan</td>
</tr>
<tr class="odd">
<td align="left">LA Galaxy</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Manchester United</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
</tbody>
</table>

The semi\_join function returns all rows from tdata where there are
matching values in **Club** with pdata, but the semi\_join keeps just
the columns from x. Once again, the exception here is that not all data
from tdata is retained because there are entries in **Club** from tdata
that do not exist in pdata.

### full\_join(y,x)

    fj <- full_join(tdata, pdata)

    ## Joining, by = "Club"

    knitr::kable(fj[1:20,], caption = "Full join between tdata (x) and pdata (y)")

<table>
<caption>Full join between tdata (x) and pdata (y)</caption>
<thead>
<tr class="header">
<th align="left">Club</th>
<th align="left">League</th>
<th align="left">League_Country</th>
<th align="left">First_Name</th>
<th align="left">Last_Name</th>
<th align="left">National_Team</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">AC Milan</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">NA</td>
</tr>
<tr class="even">
<td align="left">Ajax Amsterdam</td>
<td align="left">Eredivisie</td>
<td align="left">Netherlands</td>
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">NA</td>
</tr>
<tr class="odd">
<td align="left">Al Sadd SC</td>
<td align="left">Qatar Stars League</td>
<td align="left">Qatar</td>
<td align="left">Xavi</td>
<td align="left">Hernandez</td>
<td align="left">Spain</td>
</tr>
<tr class="even">
<td align="left">Al-Ahli Saudi FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Omar</td>
<td align="left">Al Somah</td>
<td align="left">Syria</td>
</tr>
<tr class="odd">
<td align="left">Al-Ahli Saudi FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Paulo</td>
<td align="left">Diaz</td>
<td align="left">Chile</td>
</tr>
<tr class="even">
<td align="left">Al-Ahly SC</td>
<td align="left">Egyptian Premier League</td>
<td align="left">Egypt</td>
<td align="left">Salah</td>
<td align="left">Mohsen</td>
<td align="left">Egypt</td>
</tr>
<tr class="odd">
<td align="left">Al-Ain FC</td>
<td align="left">Arabian Gulf League</td>
<td align="left">United Arab Emirates</td>
<td align="left">Marcus</td>
<td align="left">Berg</td>
<td align="left">Sweden</td>
</tr>
<tr class="even">
<td align="left">Al-Hilal FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Bafetimbi</td>
<td align="left">Gomis</td>
<td align="left">France</td>
</tr>
<tr class="odd">
<td align="left">Al-Hilal FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Carlos</td>
<td align="left">Eduardo</td>
<td align="left">Brazil</td>
</tr>
<tr class="even">
<td align="left">Al-Ittihad Kalba SC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Balazs</td>
<td align="left">Dzusudzsak</td>
<td align="left">Hungary</td>
</tr>
<tr class="odd">
<td align="left">Al-Nassr FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Ahmed</td>
<td align="left">Musa</td>
<td align="left">Nigeria</td>
</tr>
<tr class="even">
<td align="left">Al-Nassr FC</td>
<td align="left">Saudi Pro League</td>
<td align="left">Saudi Arabia</td>
<td align="left">Nordin</td>
<td align="left">Amrabat</td>
<td align="left">Morocco</td>
</tr>
<tr class="odd">
<td align="left">Arsenal FC</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">NA</td>
</tr>
<tr class="even">
<td align="left">Atlanta United</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
<td align="left">Ezequiel</td>
<td align="left">Barco</td>
<td align="left">Argentina</td>
</tr>
<tr class="odd">
<td align="left">Atlanta United</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
<td align="left">Josef</td>
<td align="left">Martinez</td>
<td align="left">Venezuela</td>
</tr>
<tr class="even">
<td align="left">Atlanta United</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
<td align="left">Miguel</td>
<td align="left">Almiron</td>
<td align="left">Paraguay</td>
</tr>
<tr class="odd">
<td align="left">Atletico Paranaense</td>
<td align="left">Brasileirao Serie A</td>
<td align="left">Brazil</td>
<td align="left">Lucho</td>
<td align="left">Gonzalez</td>
<td align="left">Argentina</td>
</tr>
<tr class="even">
<td align="left">Bate Borisov</td>
<td align="left">Belarusian Premier League</td>
<td align="left">Belarus</td>
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">NA</td>
</tr>
<tr class="odd">
<td align="left">Bayern Munich</td>
<td align="left">Bundesliga</td>
<td align="left">Germany</td>
<td align="left">NA</td>
<td align="left">NA</td>
<td align="left">NA</td>
</tr>
<tr class="even">
<td align="left">Boca Juniors</td>
<td align="left">Argentine Primera Division</td>
<td align="left">Argentina</td>
<td align="left">Carlos</td>
<td align="left">Tevez</td>
<td align="left">Argentina</td>
</tr>
</tbody>
</table>

The full\_join function return all possible combinations (all rows and
all columns from both tdata and pdata). When there are no matching
values for **Club** between tdata and pdata, the table returns NA for
whichever variable has missing entries from pdata.

### anti\_join(y,x)

    aj <- anti_join(tdata, pdata)

    ## Joining, by = "Club"

    knitr::kable(aj[1:17,], caption = "Anti join between tdata (x) and pdata (y)")

<table>
<caption>Anti join between tdata (x) and pdata (y)</caption>
<thead>
<tr class="header">
<th align="left">Club</th>
<th align="left">League</th>
<th align="left">League_Country</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">AC Milan</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="even">
<td align="left">Ajax Amsterdam</td>
<td align="left">Eredivisie</td>
<td align="left">Netherlands</td>
</tr>
<tr class="odd">
<td align="left">Arsenal FC</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="even">
<td align="left">Bate Borisov</td>
<td align="left">Belarusian Premier League</td>
<td align="left">Belarus</td>
</tr>
<tr class="odd">
<td align="left">Bayern Munich</td>
<td align="left">Bundesliga</td>
<td align="left">Germany</td>
</tr>
<tr class="even">
<td align="left">Club Brugge</td>
<td align="left">Belgian Pro League</td>
<td align="left">Belgium</td>
</tr>
<tr class="odd">
<td align="left">CSKA Moscow</td>
<td align="left">Russian Premier League</td>
<td align="left">Russain</td>
</tr>
<tr class="even">
<td align="left">Feyenoord</td>
<td align="left">Eredivisie</td>
<td align="left">Netherlands</td>
</tr>
<tr class="odd">
<td align="left">Inter Milan</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="even">
<td align="left">Manchester City</td>
<td align="left">English Premier League</td>
<td align="left">England</td>
</tr>
<tr class="odd">
<td align="left">Napoli SSC</td>
<td align="left">Serie A</td>
<td align="left">Italy</td>
</tr>
<tr class="even">
<td align="left">Olympique Lyonnais</td>
<td align="left">Ligue 1</td>
<td align="left">France</td>
</tr>
<tr class="odd">
<td align="left">Olympique Marseille</td>
<td align="left">Ligue 1</td>
<td align="left">France</td>
</tr>
<tr class="even">
<td align="left">Schalke 04</td>
<td align="left">Bundesliga</td>
<td align="left">Germany</td>
</tr>
<tr class="odd">
<td align="left">Seattle Sounders</td>
<td align="left">Major League Soccer</td>
<td align="left">United States</td>
</tr>
<tr class="even">
<td align="left">Sevilla</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
<tr class="odd">
<td align="left">Valencia CF</td>
<td align="left">La Liga</td>
<td align="left">Spain</td>
</tr>
</tbody>
</table>

Anti\_join returns all rows from tdata where there are **non-matching**
values for **Club** in pdata, and keeps only just the columns from
pdata. This output returned 17 rows, because there are 17 clubs from
tdata which do not exist in the Club variable from tdata.

### Argument Order for Join Functions

Overall, order matters when using the join functions. As evidenced by
some of the join prompts above, the ordering of datasets in the argument
field can create differing output and also affects the ordering of the
variables from the output
