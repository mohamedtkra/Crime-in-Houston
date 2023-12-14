# Crime-in-Houston
---
title: "Analysis of crime in city of houston"
author: "Mohamed Tounkara"
date: "September 26, 2023"
output:
  html_notebook:
    toc: yes
    toc_depth: 4
    number_sections: yes
  pdf_document:
    toc: yes
    toc_depth: '4'
    number_sections: yes
  html_document:
    toc: yes
    toc_depth: '4'
    df_print: paged
  word_document:
    toc: yes
    toc_depth: '4'
bibliography: STAT6382.class.bib
---

```{r}
getwd()
knitr::include_graphics("/Users/mohamedtounkara/Desktop/UHD FILES/STAT 6382/Project2/houston downtown.jpg")
```
# Introduction
```{r}

```

With evolving crime issues, law enforcement needs more information than what has been available historically to assist with decisions about training, resource allocation, and crime-fighting strategies. For that reason, major law enforcement organizations recommended that the FBI retire the Summary Reporting System (SRS) to focus on the rich data captured through the Uniform Crime Reporting (UCR) Program's National Incident-Based Reporting System (NIBRS). Through the NIBRS-only data collection set to be universally adopted by law enforcement nationwide by January 1, 2021, the FBl aims to enhance the quantity, quality, and timeliness of reported crime data and to improve the methodology used for compiling, analyzing, auditing, and publishing it.
Crime is a prevalent concern in urban centers, impacting the safety, well-being, and overall quality of life of individuals and communities. Addressing crime requires a deep understanding of its patterns, trends, and underlying factors. The Crime Analysis and Prediction Project for Houston aims to unravel these complexities by employing data analysis and predictive modeling. Through this endeavor, I strive to offer meaningful insights and aid in the development of effective crime prevention strategies. the dataset includes essential information such as incident details, occurrence date and hour, offense types, and location details. 
My project revolves around the analysis of crime statistics in Houston, focusing on data from January to August 2023. My primary goals encompass crime analysis and insight extraction. Through the application of using SQL and Python the whole purpose of this project will be to detect patterns, assess crime rates, pinpoint areas prone to criminal activities and build some visualizations to highlight my findings.

For Step 1, analyzing the data is the initial and pivotal stage in my analytical process. During this phase, I will also identify any missing or erroneous data points. Moreover, I will explore potential outliers or anomalies that might distort my analysis, investigating whether they are legitimate data points or require further scrutiny and potential removal. Furthermore, I will transition to the critical task of cleaning the data. I will start by addressing missing data through imputation or removal, depending on the nature and extent of the gaps. Additionally, I will rectify any data entry errors, duplications, or inaccuracies that might have been unearthed during the analysis.

In Step2, one of the primary objectives of this analysis is to identify the most prevalent types of crimes occurring in the city of Houston. By carefully analyzing the dataset, I will extract insights into the frequency and occurrence of various types of crimes. Understanding the geographical distribution of crime is essential for effective law enforcement and community engagement. In this part of the analysis, I will focus on identifying the zip codes within Houston that witness the highest incidence of criminal activity. Finally, the temporal aspect of crime data allows me to unravel patterns based on day, month, or even hour, which help law enforcement anticipate and proactively address potential surges in criminal incidents. Whether it's planning patrols or implementing public safety campaigns, this understanding of temporal trends is invaluable for effective crime prevention and management.

Finally, on Step 3 I will proceed to data visualization to highlight the current and forecast future trends 

# Overview 

------------------------------------------------------------

*  Step 1: Explanatory analysis

    * 1a: Data structure
    
    * 1b: Data Cleaning

*  Step 2: Exploratory analysis

    * 2a: Most common crimes based on occurrence
    
    * 2b: Description of types of crime
    
    * 2c: Zip where most crimes occur

    * 2d: Time where most crimes happened
  
*  Step 3: Visualization
  
    * 3a: Visualization showing current trend
    
    * 3b: Visualization showing general trend 
  
    * 3c: Visualization showing forecasted trend for the remaining of the year 2023.
    
# Explanatory Analysis
During this part, I will gain insights, discover patterns, and understand the underlying structure of the dataset. 

## Data structure 
Our dataset (Houstoncrime) has 168,651 rows and 16 columns. For this particular analysis I am going to dive into further analysis during the data cleaning which will improve the quality of my dataset and as a result overall analysis

```{r}
# Load necessary libraries
library(readxl)
library(knitr)

# Rlibrary(readr)
library(readr)
Houstoncrime <- read_csv("Houstoncrime.csv")
Houstoncrime
```

```{r}
# Display the first 10 rows
head(Houstoncrime, 10)
```

## Data Cleaning
First, I decided to remove few columns as they are not necessary (Incident, Suffix) for my analysis

```{r}
#Removing few columns (Incident, Suffix, )
df1 <- Houstoncrime[, !(names(Houstoncrime) %in% c('Incident', 'Suffix'))]
df1
```
Here I will show the different columns and the number of missing values that they may have. As we can see it: 
Beat has 150 missing values, StreetNo has 587, StreetType has 12323 and zip code has 2301 values.

```{r}
#Identify missing value in the data
missing_values_count <- colSums(is.na(df1))
missing_values_count
```
After identifying the count of missing value, I will start to remove them. As we can it, the number of rows went from 168,651 to 154,142 rows.

```{r}
# Removing missing rows
df2 <- df1[complete.cases(df1), ]
df2
```
After rechecking we can see now that the dataset doesn't have any missing value so we can proceed to the next step of the analysis.

```{r}
#Identify missing value in the data
missing_values_count1 <- colSums(is.na(df2))
missing_values_count1
```
# Exploratory analysis
Here I will explore the data using mainly SQL to discover insights that can guide further analysis decision-making.
Here we have a breakdown of the different types of crime:
```{r}
getwd()
knitr::include_graphics("/Users/mohamedtounkara/Desktop/UHD FILES/STAT 6382/Project2/NIBRSDescription.png")
```
I then decided to run a query in my python script to have a breakdown of the most common types of crimes.
```{r}
getwd()
knitr::include_graphics("/Users/mohamedtounkara/Desktop/UHD FILES/STAT 6382/Project2/Top10crimes.png")
```
## Description of types of crimes
Theft from motor vehicle as the name suggests, pertains to stealing items or property that are located inside a motor vehicle. This offense involves breaking into the vehicle or unlawfully accessing its interior with the intent to pilfer valuables. The items targeted for theft can range from personal belongings like wallets, purses, laptops, electronic devices, to tools or any valuable possessions left inside the car. This crime focuses on the theft of objects from the vehicle rather than the theft of the vehicle itself.

Simple assault: According to Texas Penal Code 22.01, Simple Assault [@SimpleAssault] is when the offender causes bodily harm to another person, threatening to cause bodily harm with the intention of doing so.Furthermore, simple assault when the offender intentionally or recklessly causes bodily injury would be considered a Class A misdemeanor. 

Intimidation: is an act or course of conduct directed at a specific person to cause that person to fear or apprehend fear. Usually, an individual intimidates others by deterring or coercing them to take an action they do not want to take. The intimidation may become a civil or criminal offense unless that behavior serves a “legitimate purpose.” [@Intimidation]

All other larceny: Larceny is a legal term for theft, which involves the unlawful taking of someone else's personal property with the intent to permanently deprive them of it. However, not all thefts fit neatly into subcategories like motor vehicle theft, shoplifting, or burglary."All other larceny" serves as a catch-all category to encompass thefts that don't have their own distinct category but are still considered theft or larceny under the law. 

Aggravated Assault: Aggravated Assault consists of intentionally, knowingly, or recklessly causes bodily injury to another, including the person's spouse; intentionally or knowingly threatens another with imminent bodily injury, including the person's spouse; or intentionally or knowingly causes physical contact with another when the person knows or should reasonably believe that the other will regard the contact as offensive or provocative [@Assault].

## Crimes breakdown
```{r}
library(readr)
Crime <- read_csv("~/Desktop/UHD FILES/STAT 6382/Project2/Crime breakdown.csv")
Crime

```


```{r}
# Load the necessary libraries
#install.packages("leaflet")
library(leaflet)
library(dplyr)
# Categorize the ZIP codes based on the crime counts into three groups (low, mid, and high)
Crime <- Crime %>%
  mutate(Category = case_when(
    Total > 2000 ~ "High",
    Total >= 1000 & Total <= 1999 ~ "Mid",
    Total < 1000 ~ "Low",
    TRUE ~ "Unknown"
  ))
Crime
```
Here I used the following zipcode and went to [@Zip] where I was able to enter the zip codes and have the corresponding longitude and latitude. The longitude and latitude helped me to build the city of houston map with a highlight of the most affected area affected by crimes.

```{r}
# Create a data frame with ZIP codes and their coordinates
zip_coordinates <- data.frame(
  ZIPCode = c(
    "77036", "77092", "77002", "77004", "77063", "77057", "77021", "77007", "77072", "77022",
    "77077", "77056", "77060", "77042", "77081", "77016", "77099", "77061", "77054", "77055",
    "77008", "77006", "77032", "77009", "77093", "77080", "77033", "77026", "77074", "77040",
    "77082", "77034", "77088", "77075", "77091", "77076", "77017", "77035", "77051", "77027",
    "77019", "77087", "77020", "77096", "77045", "77028", "77023", "77003", "77024", "77079",
    "77018", "77025", "77098", "77013", "77043", "77089", "77067", "77031", "77070", "77048",
    "77015", "77071", "77011", "77012", "77037", "77078", "77030", "77047", "77053", "77598",
    "77339", "77085", "77041", "77029", "77396", "77546", "77084", "77489", "77005", "77064",
    "77058", "77062", "77345", "77094", "77338", "77010", "77038", "77336", "77059", "77014",
    "77049", "77046", "77083", "77477", "77050", "77044", "77365", "77504", "77066", "77401",
    "77069", "77571", "77587", "77039", "77090", "77086", "77449", "77478", "77520", "77581",
    "77584", "77346", "77065", "77502", "77388", "77530", "77450", "77494", "77301", "77471",
    "77532", "77373", "77461", "77539", "77095", "77573", "77521", "77536", "77562", "77479",
    "77423", "77386", "77318", "77025-0000", "77444", "77511", "77485", "77068", "77429",
    "77377", "77018-2138", "77379", "77043-1115"
  ),
  Latitude = c(
    # Fill in the corresponding latitude values
    29.678133, 29.827150, 29.756845, 29.729470, 29.732755, 29.743525, 29.716179, 29.761558, 29.830198, 29.828654,
    29.785727, 29.732968, 29.917235, 29.722861, 29.714814, 29.839291, 29.857083, 29.834102, 29.780178, 29.679442,
    29.842276, 29.789579, 29.810665, 29.864345, 29.801456, 29.874051, 29.688337, 29.710015, 29.742041, 29.743679,
    29.821856, 29.829774, 29.646645, 29.673600, 29.669758, 29.637051, 29.822232, 29.635571, 29.814030, 29.635997,
    29.651927, 29.825201, 29.735747, 29.731068, 29.824727, 29.814014, 29.710610, 29.818715, 29.920793, 29.813490,
    29.767273, 29.650534, 29.753925, 29.640294, 29.656306, 29.729134, 29.610696, 29.695061, 29.661352, 29.625555,
    29.760452, 29.957356, 29.931171, 29.857562, 29.802982, 29.797797, 29.801430, 29.741949, 29.801920, 29.703653,
    29.826216, 29.771498, 29.633032, 30.144077, 30.148100, 29.782371, 30.133276, 29.870622, 29.581684, 30.116333,
    29.712850, 29.834758, 29.958723, 30.059794, 30.052960, 29.757054, 30.034752, 29.773164, 29.735577, 30.123491,
    29.818051, 30.138713, 29.894143, 29.780000, 29.752632, 30.021058, 30.019868, 29.652686, 30.168924, 29.922055,
    30.154881, 30.164155, 30.101544, 30.051791, 30.232476, 29.836800, 30.189029, 30.131871, 30.011755, 29.905380,
    30.101818, 30.269407, 30.037224, 29.843594, 29.843943, 29.905927, 29.992579, 29.953725, 30.096200, 30.019623,
    30.135055, 29.882168, 30.100098, 30.242171, 29.628766, 29.819729, 30.125205, 29.866793, 30.215761, 29.672654,
    30.244769, 29.776740, 30.269997, 29.879508, 30.131282, 29.832692, 30.163479, 29.736216, 30.045722, 29.548767,
    30.258698, 29.922056, 30.108183
  ),
  Longitude = c(
    # Fill in the corresponding longitude values
    -95.549542, -95.459126, -95.367650, -95.365820, -95.543183, -95.490574, -95.367216, -95.368450, -95.474718, -95.312597,
    -95.571376, -95.469450, -95.422317, -95.496614, -95.408998, -95.299646, -95.313425, -95.523949, -95.424119, -95.443977,
    -95.419372, -95.396394, -95.276337, -95.382891, -95.291808, -95.558200, -95.295428, -95.369489, -95.351700, -95.522991,
    -95.430830, -95.266154, -95.348402, -95.269675, -95.481399, -95.358286, -95.218836, -95.554746, -95.360018, -95.289762,
    -95.304872, -95.395565, -95.342587, -95.235527, -95.282252, -95.467894, -95.421809, -95.481163, -95.257966, -95.451744,
    -95.389258, -95.543171, -95.322034, -95.470679, -95.394541, -95.525935, -95.252825, -95.330927, -95.236487, -95.374996,
    -95.542823, -95.410935, -95.048935, -95.072978, -95.263294, -95.371253, -94.936739, -95.112888, -95.166527, -95.532939,
    -95.423762, -95.291153, -95.495503, -95.536684, -95.579957, -95.208879, -95.463090, -95.432385, -95.216751, -95.459444,
    -95.543512, -95.503394, -95.185747, -95.345786, -95.148751, -95.124422, -95.137179, -95.360186, -94.897717, -95.207875,
    -95.164407, -95.195392, -95.378135, -95.183036, -95.359422, -95.309583, -95.063653, -95.128375, -95.345866, -95.136252,
    -95.218587, -95.137688, -95.087819, -95.177394, -95.201874, -95.178785, -95.097070, -95.287151, -95.183337, -95.129634,
    -95.197876, -95.032115, -95.045065, -95.166257, -95.179936, -95.315068, -95.182911, -95.128677, -95.004148, -95.294087,
    -95.091192, -95.131124, -95.065505, -95.139785, -95.077806, -95.025651, -95.049527, -95.160804, -95.158359, -94.889596,
    -95.037931, -95.064579, -94.981964, -94.976356, -95.002302, -95.061166, -95.093085, -94.972271, -95.009343, -94.997890,
    -95.303752, -94.974112, -94.960286
  )
)

```

```{r}
# Merge the Crime data frame with ZIP code coordinates
Crime1 <- merge(Crime, zip_coordinates, by = "ZIPCode", all.x = TRUE)

# Check the first few rows of the merged data frame
head(Crime1)

```

I am focusing on the top 30 zip codes here to make it easy to visualize the zip code
```{r}
# Define the selected ZIP codes
selected_zip_codes <- c(
    "77036", "77092", "77002", "77004", "77063", "77057", "77021", "77007", "77072", "77022",
    "77077", "77056", "77060", "77042", "77081", "77016", "77099", "77061", "77054", "77055",
    "77008", "77006", "77032", "77009", "77093", "77080", "77033", "77026", "77074", "77040"
)

# Define fictional latitude and longitude values for the selected ZIP codes
latitude_values <- c(
    29.704343, 29.830132, 29.759730, 29.724440, 29.699288, 29.748334, 29.685975, 29.769119, 29.830748, 29.813946,
    29.743500, 29.722198, 29.950942, 29.678488, 29.702268, 29.665596, 29.647915, 29.701972, 29.643774, 29.654915,
    29.802720, 29.774455, 29.734303, 29.881051, 29.786492, 29.855871, 29.719308, 29.681129, 29.753054, 29.817698
)

longitude_values <- c(
    -95.558573, -95.471472, -95.367062, -95.365084, -95.564399, -95.488007, -95.363071, -95.401716, -95.484957, -95.303845,
    -95.568900, -95.458210, -95.340129, -95.471966, -95.520820, -95.332171, -95.288065, -95.293130, -95.318143, -95.534384,
    -95.412013, -95.393520, -95.320114, -95.165989, -95.143940, -95.474919, -95.308219, -95.378126, -95.369188, -95.442305
)

# Define the Total column values
total_values <- c(
    2481, 1845, 1518, 1439, 1436, 1402, 1396, 1394, 1331, 1310, 1284, 1263, 1192, 1128, 1114, 1097, 1055, 1045, 1028,
    988, 959, 946, 938, 932, 915, 909, 906, 895, 890, 882
)

# Create a data frame with the ZIP codes, coordinates, and Total values
zip_coordinates <- data.frame(
    ZIPCode = selected_zip_codes,
    Latitude = latitude_values,
    Longitude = longitude_values,
    Total = total_values
)

```

Here I decidded to divide the Zip codes based on the number of crimes committed (High, Medium and Low) and assigned a specific color (red, black and green) to each one of them.
```{r}
library(dplyr)
# Create a leaflet map with the selected ZIP codes and larger marker sizes
Zipmap <- leaflet(data = zip_coordinates) %>%
    addTiles() %>%
    addCircleMarkers(
        radius = ~ifelse(Total > 2000, 15, ifelse(Total >= 1000, 10, 7)),
        color = ~ifelse(Total > 2000, "red", ifelse(Total >= 1000, "black", "green")),
        label = ~ZIPCode,
        popup = ~paste("ZIP Code: ", ZIPCode, "<br>Latitude: ", Latitude, "<br>Longitude: ", Longitude, "<br>Total Crime: ", Total),     
        popupOptions = popupOptions(keepInView = TRUE),

        lat = ~Latitude,  # Use the Latitude column for latitude
        lng = ~Longitude  # Use the Longitude column for longitude
    ) %>%
    addLegend(
        "bottomright",
        title = "Crime Categories",
        colors = c("green", "black", "red"),
        labels = c("Low (0-999)", "Mid (1000-1999)", "High (>2000)"),
        opacity = 10
    )
```

For example Zip code 77036 is in red because it has more than 2000 crimes, 77002 is in black because it has less than 2000 crimes and 77003 is in green because it has less than 1000 crimes
```{r}
getwd()
knitr::include_graphics("/Users/mohamedtounkara/Desktop/UHD FILES/STAT 6382/Project2/Overviewmap.png")
```


## Zip where Theft from motor vehicle happen
Here we can see that most theft happen in Houston and especially in the Zip: 77007, 77019, 77036, 77092 and 77056
```{r}
getwd()
knitr::include_graphics("/Users/mohamedtounkara/Desktop/UHD FILES/STAT 6382/Project2/Zipcodetheft.png")
```
Here I have a map of the area having major issue with "Theft from motor vehicle"

```{r}
# Load the necessary packages
library(leaflet)
library(dplyr)
```


```{r}
# Define the ZIP codes and their corresponding colors
zip_colors <- data.frame(
  ZIPCode = c("77007", "77019", "77036", "77092", "77056"),
  Color = c("blue", "black", "green", "violet", "red")
)

# Load your corrected dataset
data <- df2

# Convert ZIPCode in your dataset to character
data$ZIPCode <- as.character(data$ZIPCode)

# Filter the dataset to include only the specified ZIP codes
filtered_data <- data %>%
  filter(ZIPCode %in% zip_colors$ZIPCode)

# Merge the filtered data with the color information
filtered_data <- left_join(filtered_data, zip_colors, by = "ZIPCode")

```


```{r}
# Create a map
mapp <- leaflet(data = filtered_data) %>%
  addTiles() %>%
  addCircleMarkers(
    lat = ~MapLatitude, lng = ~MapLongitude,
    radius = 1,  # Adjust the marker size
    color = ~Color,  # Assign colors based on ZIP code
    label = ~ZIPCode  # Display the ZIP code as a label
  )
```

Zips 77007", "77019", "77036", "77092" and "77056" respectively in "blue", "black", "green", "violet", "red"

```{r}
getwd()
knitr::include_graphics("/Users/mohamedtounkara/Desktop/UHD FILES/STAT 6382/Project2/theftfromvehicle.png")
```

Here we have the distribution by hours of crime related to theft from a vehicle
```{r}
# Load the necessary packages
library(dplyr)

# Load your corrected dataset
data <- read.csv("Houstoncrime.csv")

# Filter the dataset to include only the specified ZIP codes and "Theft from motor vehicle" incidents
filtered_data <- data %>%
  filter(ZIPCode %in% c(77007, 77019, 77036, 77092, 77056) & NIBRSDescription == "Theft from motor vehicle")

# Group the data by ZIPCode and RMSOccurrenceHour, and count the number of occurrences
crime_counts <- filtered_data %>%
  group_by(ZIPCode, RMSOccurrenceHour) %>%
  summarize(Count = n()) %>%
  arrange(desc(Count))  # Arrange in descending order by Count
crime_counts
```

As we can see it here, most crime are happening between 6 and 7pm except for Zip Code 77036 where most crimes usually happen around 3pm.
```{r}
# Load the necessary packages
library(dplyr)

# Load your corrected dataset
data <- read.csv("Houstoncrime.csv")

# Filter the dataset to include only the specified ZIP codes and "Theft from motor vehicle" incidents
filtered_data <- data %>%
  filter(ZIPCode %in% c(77007, 77019, 77036, 77092, 77056) & NIBRSDescription == "Theft from motor vehicle")

# Group the data by ZIPCode and RMSOccurrenceHour, and count the number of occurrences
crime_counts1 <- filtered_data %>%
  group_by(ZIPCode, RMSOccurrenceHour) %>%
  summarize(Count = n()) %>%
  arrange(ZIPCode, desc(Count)) %>%  # Arrange by ZIPCode and Count in descending order
  slice(1)  # Select the highest count for each ZIPCode

# Print the results
print(crime_counts1)

```

Most crimes related  are happening in the Zip 77007, 77056, 77019 between 6 and 7pm. The particularity of these ZipCode is that average income is respectively around $98,725, $93,069 and $87,394 and median house price is $308,700, $555,700 and $378,700. The main reason for High number of crime related to Theft from a motor vehicle is  because these three ZipCodes are suburban residential areas and relatively safe and quiet. As a result, residents can become complacent about car security. They may leave their car doors unlocked or the keys in the ignition. Oftentimes, Cars in residential locations that are adjacent to lower-tier socioeconomic neighborhoods (which often have higher crime rates) are generally more vulnerable. Thieves who reside in the high-crime neighborhoods need only short commute to search for items or cars to steal. They have the advantage of being familiar with the area. In our case, Zip Code 77092 and 77036 are very close to those primes ZipCode. As on contrast, median income in both of those ZipCode are $37,314 and $28,266. In addition, crime happen around that time because it is the time most cars are present in these areas, as well as the fact that darkness provides cover for the thieves. In residential areas that contain multi-family apartment complexes.

# Breakdown by ZipCodes
```{r}
if (!require(ggplot2)) {
  install.packages("ggplot2")
  library(ggplot2)
}

# Create a single plot with facets for different ZIP codes
combined_plot <- ggplot(crime_counts, aes(x = factor(RMSOccurrenceHour, levels = 1:24), y = Count, fill = factor(ZIPCode))) +
  geom_bar(stat = "identity") +
  labs(title = "Combined Crime Occurrences by Time for Different ZIP Codes",
       x = "RMSOccurrenceHour (24-hour format)",
       y = "Count") +
  scale_x_discrete(breaks = 1:24, labels = 1:24) +  # Set breaks and labels for the x-axis
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1),
        legend.position = "bottom") +
  facet_wrap(~ZIPCode, ncol = 5)  # Split into 5 columns

# Print the combined plot
print(combined_plot)
```
```{r}
if (!require(ggplot2)) {
  install.packages("ggplot2")
  library(ggplot2)
}

# Filter the dataset for ZIPCode 77007
subset_data <- subset(crime_counts, ZIPCode == 77007)

# Create a bar chart to show the distribution of crime occurrences by time based on the Count column
ggplot(subset_data, aes(x = factor(RMSOccurrenceHour, levels = 1:24), y = Count)) +
  geom_bar(stat = "identity", fill = "blue") +
  labs(title = "Crime Occurrences in ZIPCode 77007 by Time",
       x = "RMSOccurrenceHour (24-hour format)",
       y = "Count") +
  scale_x_discrete(limits = 0:23) +  # Set x-axis to include all hours from 1 to 24
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))  

```

```{r}
if (!require(ggplot2)) {
  install.packages("ggplot2")
  library(ggplot2)
}

# Filter the dataset for ZIPCode 77019
subset_data <- subset(crime_counts, ZIPCode == 77019)

# Create a bar chart to show the distribution of crime occurrences by time based on the Count column
ggplot(subset_data, aes(x = factor(RMSOccurrenceHour, levels = 1:24), y = Count)) +
  geom_bar(stat = "identity", fill = "red") +
  labs(title = "Crime Occurrences in ZIPCode 77019 by Time",
       x = "RMSOccurrenceHour (24-hour format)",
       y = "Count") +
  scale_x_discrete(limits = 0:23) +  # Set x-axis to include all hours from 1 to 24
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))

```
```{r}
if (!require(ggplot2)) {
  install.packages("ggplot2")
  library(ggplot2)
}

# Filter the dataset for ZIPCode 77036
subset_data_77036 <- subset(crime_counts, ZIPCode == 77036)

# Filter the dataset for ZIPCode 77036
subset_data <- subset(crime_counts, ZIPCode == 77036)

# Create a bar chart to show the distribution of crime occurrences by time based on the Count column
ggplot(subset_data, aes(x = factor(RMSOccurrenceHour, levels = 1:24), y = Count)) +
  geom_bar(stat = "identity", fill = "black") +
  labs(title = "Crime Occurrences in ZIPCode 77036 by Time",
       x = "RMSOccurrenceHour (24-hour format)",
       y = "Count") +
  scale_x_discrete(limits = 0:24) +  # Set x-axis to include all hours from 1 to 24
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))  

```
```{r}
if (!require(ggplot2)) {
  install.packages("ggplot2")
  library(ggplot2)
}

# Filter the dataset for ZIPCode 77056
subset_data_77056 <- subset(crime_counts, ZIPCode == 77056)

# Create a bar chart to show the distribution of crime occurrences by time based on the Count column
ggplot(subset_data_77056, aes(x = factor(RMSOccurrenceHour, levels = 1:24), y = Count)) +
  geom_bar(stat = "identity", fill = "green") +
  labs(title = "Crime Occurrences in ZIPCode 77056 by Time",
       x = "RMSOccurrenceHour (24-hour format)",
       y = "Count") +
  scale_x_discrete(limits = 0:24) +  # Set x-axis to include all hours from 1 to 24
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))

```

```{r}
if (!require(ggplot2)) {
  install.packages("ggplot2")
  library(ggplot2)
}

# Filter the dataset for ZIPCode 77092
subset_data_77092 <- subset(crime_counts, ZIPCode == 77092)

# Create a bar chart to show the distribution of crime occurrences by time based on the Count column
ggplot(subset_data_77092, aes(x = factor(RMSOccurrenceHour, levels = 1:24), y = Count)) +
  geom_bar(stat = "identity", fill = "brown") +
  labs(title = "Crime Occurrences in ZIPCode 77092 by Time",
       x = "RMSOccurrenceHour (24-hour format)",
       y = "Count") +
  scale_x_discrete(limits = 0:24) +  # Set x-axis to include all hours from 1 to 24
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))

```

```{r}
# Load the necessary packages
library(ggplot2)

# Create a data frame
data1 <- data.frame(
  ZipCode = c(77007, 77019, 77036, 77056, 77092),
  RMSOccurrenceHour = c(19, 19, 15, 19, 18),
  Count = c(124, 76, 59, 78, 62)
)

# Create a histogram for the provided data
histogram_plot <- ggplot(data1, aes(x = RMSOccurrenceHour, y = Count, fill = factor(ZipCode))) +
  geom_bar(stat = "identity", position = "dodge") +
  labs(
    title = "Theft Distribution Based on Time by ZipCode",
    x = "RMS Occurrence Hour",
    y = "Count"
  ) +
  scale_fill_discrete(name = "ZipCode")

# Display the histogram
print(histogram_plot)


```
Here I am focusing on the second most common type of crime which is "Simple Assault"
```{r}
getwd()
knitr::include_graphics("/Users/mohamedtounkara/Desktop/UHD FILES/STAT 6382/Project2/SimpleAssault.png")
```
Here I have a map of the areas having major issue with crimes categorized as Simple Assault

```{r}
# Load the necessary packages
library(leaflet)
library(dplyr)
```


```{r}
# Define the ZIP codes and their corresponding colors
zip_colors1 <- data.frame(
  ZIPCode = c("77036", "77021", "77002", "77088", "77057"),
  Color = c("blue", "black", "green", "violet", "red")
)

# Load your corrected dataset
data <- df2

# Convert ZIPCode in your color data frame to numeric
zip_colors1$ZIPCode <- as.numeric(zip_colors1$ZIPCode)

# Filter the dataset to include only the specified ZIP codes
filtered_data1 <- data %>%
  filter(ZIPCode %in% zip_colors1$ZIPCode)

# Merge the filtered data with the color information
filtered_data <- left_join(filtered_data1, zip_colors1, by = "ZIPCode")

```

```{r}
# Create a map
mapp <- leaflet(data = filtered_data) %>%
  addTiles() %>%
  addCircleMarkers(
    lat = ~MapLatitude, lng = ~MapLongitude,
    radius = 1,  # Adjust the marker size
    color = ~Color,  # Assign colors based on ZIP code
    label = ~ZIPCode  # Display the ZIP code as a label
  )

```

The zip codes are "77036", "77021", "77002", "77088", "77057" and respective Colors are "blue", "black", "green", "violet", "red"
```{r}
getwd()
knitr::include_graphics("/Users/mohamedtounkara/Desktop/UHD FILES/STAT 6382/Project2/simpleassaultmap.png")
```

```{r}
# Load the necessary packages
library(dplyr)

# Load your corrected dataset
data <- df2

# Filter the dataset to include only the specified ZIP codes and "Simple assault" incidents
filtered_data <- data %>%
  filter(ZIPCode %in% c(77036, 77021, 77002, 77088, 77057) & NIBRSDescription == "Simple assault")

# Group the data by ZIPCode and RMSOccurrenceHour, and count the number of occurrences
crime_counts11 <- filtered_data %>%
  group_by(ZIPCode, RMSOccurrenceHour) %>%
  summarize(Count = n()) %>%
  arrange(desc(Count))  # Arrange in descending order by Count
crime_counts11
```
As we can see it here, most crime are happening between 11pm and 2am except for Zip Code 77088 and 77021 where most crimes usually happen around 5 and 6pm.
The main reasons for these type of crimes is due to: Nightlife and Social Activities. During these hours, many people are engaged in social activities, such as going to bars, clubs, parties, or late-night gatherings. Increased social interactions can sometimes lead to conflicts or disputes, which may escalate into simple assaults.
Another factor is Alcohol and Substance use. Alcohol and drug use tend to be higher during late-night hours, and the impaired judgment and disinhibition that can result from these substances may contribute to aggressive behavior and conflicts.


```{r}
# Load the necessary packages
library(dplyr)

# Load your corrected dataset
data <- df2

# Filter the dataset to include only the specified ZIP codes and "Theft from motor vehicle" incidents
filtered_data <- data %>%
  filter(ZIPCode %in% c(77036, 77021, 77002, 77088, 77057) & NIBRSDescription == "Simple assault")

# Group the data by ZIPCode and RMSOccurrenceHour, and count the number of occurrences
crime_counts111 <- filtered_data %>%
  group_by(ZIPCode, RMSOccurrenceHour) %>%
  summarize(Count = n()) %>%
  arrange(ZIPCode, desc(Count)) %>%  # Arrange by ZIPCode and Count in descending order
  slice(1)  # Select the highest count for each ZIPCode

# Print the results
print(crime_counts111)
```
Breakdown by zip codes

```{r}
if (!require(ggplot2)) {
  install.packages("ggplot2")
  library(ggplot2)
}
# Create a single plot with facets for different ZIP codes
combined_plot <- ggplot(crime_counts11, aes(x = factor(RMSOccurrenceHour, levels = 1:24), y = Count, fill = factor(ZIPCode))) +
  geom_bar(stat = "identity") +
  labs(title = "Combined Crime Occurrences by Time for Different ZIP Codes",
       x = "RMSOccurrenceHour (24-hour format)",
       y = "Count") +
  scale_x_discrete(breaks = 1:24, labels = 1:24) +  # Set breaks and labels for the x-axis
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1),
        legend.position = "bottom") +
  facet_wrap(~ZIPCode, ncol = 5)  # Split into 5 columns

# Print the combined plot
print(combined_plot)
```
Here is a bgraph showing the distribution per hour
```{r}
# Load the necessary packages
library(dplyr)
library(ggplot2)

# Load your corrected dataset
data <- df2

# Filter the dataset to include only "Simple assault" incidents in ZIP Code 77036
filtered_data <- data %>%
  filter(ZIPCode == 77036 & NIBRSDescription == "Simple assault")

# Create a bar chart of the distribution of "Simple assault" crimes
ggplot(filtered_data, aes(x = RMSOccurrenceHour)) +
  geom_bar(fill = "blue") +
  labs(title = "Distribution of Simple Assault Crimes in ZIP Code 77036",
       x = "Hour of Occurrence",
       y = "Count")

```

```{r}
# Load the necessary packages
library(dplyr)
library(ggplot2)

# Load your corrected dataset
data <- df2

# Filter the dataset to include only "Simple assault" incidents in ZIP Code 77057
filtered_data <- data %>%
  filter(ZIPCode == 77057 & NIBRSDescription == "Simple assault")

# Create a bar chart of the distribution of "Simple assault" crimes
ggplot(filtered_data, aes(x = RMSOccurrenceHour)) +
  geom_bar(fill = "black") +
  labs(title = "Distribution of Simple Assault Crimes in ZIP Code 77057",
       x = "Hour of Occurrence",
       y = "Count")

```

```{r}
# Load the necessary packages
library(dplyr)
library(ggplot2)

# Assuming you've already loaded your dataset df2
data <- df2

# Filter the dataset to include only "Simple assault" incidents in ZIP Code 77002
filtered_data <- data %>%
  filter(ZIPCode == 77002 & NIBRSDescription == "Simple assault")

# Create a bar chart of the distribution of "Simple assault" crimes
ggplot(filtered_data, aes(x = RMSOccurrenceHour)) +
  geom_bar(fill = "red") +
  labs(title = "Distribution of Simple Assault Crimes in ZIP Code 77002",
       x = "Hour of Occurrence",
       y = "Count")

```
```{r}
# Load the necessary packages
library(dplyr)
library(ggplot2)

# Assuming you've already loaded your dataset df2
data <- df2

# Filter the dataset to include only "Simple assault" incidents in ZIP Code 77021
filtered_data <- data %>%
  filter(ZIPCode == 77021 & NIBRSDescription == "Simple assault")

# Create a bar chart of the distribution of "Simple assault" crimes in ZIP Code 77021
ggplot(filtered_data, aes(x = RMSOccurrenceHour)) +
  geom_bar(fill = "darkgreen") +
  labs(title = "Distribution of Simple Assault Crimes in ZIP Code 77021",
       x = "Hour of Occurrence",
       y = "Count")

```

```{r}
# Load the necessary packages
library(dplyr)
library(ggplot2)

# Assuming you've already loaded your dataset df2
data <- df2

# Filter the dataset to include only "Simple assault" incidents in ZIP Code 77088
filtered_data <- data %>%
  filter(ZIPCode == 77088 & NIBRSDescription == "Simple assault")

# Create a bar chart of the distribution of "Simple assault" crimes in ZIP Code 77088
ggplot(filtered_data, aes(x = RMSOccurrenceHour)) +
  geom_bar(fill = "darkred") +
  labs(title = "Distribution of Simple Assault Crimes in ZIP Code 77088",
       x = "Hour of Occurrence",
       y = "Count")

```
Now I will focus on Zip where Destruction, damage and vandalism happen the most

```{r}
getwd()
knitr::include_graphics("/Users/mohamedtounkara/Desktop/UHD FILES/STAT 6382/Project2/Destruction.png")
```

```{r}
# Define the ZIP codes and their corresponding colors
zip_colors11 <- data.frame(
  ZIPCode = c("77036", "77057", "77042", "77060", "77072"),
  Color = c("blue", "black", "green", "violet", "red")
)

# Assuming you've already loaded your dataset df2
data <- df2

# Convert ZIPCode in your color data frame to character
zip_colors11$ZIPCode <- as.character(zip_colors11$ZIPCode)

# Convert ZIPCode in your main dataset to character (if it's not already)
data$ZIPCode <- as.character(data$ZIPCode)

# Filter the dataset to include only the specified ZIP codes
filtered_data11 <- data %>%
  filter(ZIPCode %in% zip_colors11$ZIPCode)

# Merge the filtered data with the color information
filtered_data111 <- left_join(filtered_data11, zip_colors11, by = "ZIPCode")

```

```{r}
# Create a map
mappp <- leaflet(data = filtered_data111) %>%
  addTiles() %>%
  addCircleMarkers(
    lat = ~MapLatitude, lng = ~MapLongitude,
    radius = 1,  # Adjust the marker size
    color = ~Color,  # Assign colors based on ZIP code
    label = ~ZIPCode  # Display the ZIP code as a label
  )
```

On this map we have zip code "77036", "77057", "77042", "77060", "77072" with color "blue", "black", "green", "violet", "red"
```{r}
getwd()
knitr::include_graphics("/Users/mohamedtounkara/Desktop/UHD FILES/STAT 6382/Project2/Destructionmap.png")
```

```{r}
# Load the necessary packages
library(dplyr)

# Load your corrected dataset
data <- df2

# Filter the dataset to include only the specified ZIP codes and "Destruction, damage, and vandalism" incidents
filtered_data2 <- data %>%
  filter(ZIPCode %in% c("77036", "77057", "77042", "77060", "77072") & NIBRSDescription == "Destruction, damage, vandalism")
filtered_data2
```
Here is the distribution per hour of crimes related to Destruction 
```{r}
 
# Group the data by ZIPCode and RMSOccurrenceHour, and count the number of occurrences
crime_counts2 <- filtered_data2 %>%
  group_by(ZIPCode, RMSOccurrenceHour) %>%
  summarize(Count = n()) %>%
  arrange(desc(Count))  # Arrange in descending order by Count
crime_counts2
```
```{r}
# Load the necessary packages
library(dplyr)

# Load your corrected dataset
data <- df2

# Group the data by ZIPCode and RMSOccurrenceHour, and count the number of occurrences
crime_counts3 <- filtered_data2 %>%
  group_by(ZIPCode, RMSOccurrenceHour) %>%
  summarize(Count = n()) %>%
  arrange(ZIPCode, desc(Count)) %>%  # Arrange by ZIPCode and Count in descending order
  slice(1)  # Select the highest count for each ZIPCode

# Print the results
print(crime_counts3)
```

```{r}
if (!require(ggplot2)) {
  install.packages("ggplot2")
  library(ggplot2)
}

# Create a single plot with facets for different ZIP codes
combined_plot <- ggplot(crime_counts2, aes(x = factor(RMSOccurrenceHour, levels = 1:24), y = Count, fill = factor(ZIPCode))) +
  geom_bar(stat = "identity") +
  labs(title = "Combined Crime Occurrences by Time for Different ZIP Codes",
       x = "RMSOccurrenceHour (24-hour format)",
       y = "Count") +
  scale_x_discrete(breaks = 1:24, labels = 1:24) +  # Set breaks and labels for the x-axis
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1),
        legend.position = "bottom") +
  facet_wrap(~ZIPCode, ncol = 5)  # Split into 5 columns

# Print the combined plot
print(combined_plot)
```

```{r}
# Load the necessary packages
library(dplyr)
library(ggplot2)

# Load your corrected dataset
data <- df2

# Filter the dataset to include only "Destruction, damage, vandalism" incidents in ZIP Code 77036
filtered_data <- data %>%
  filter(ZIPCode == 77036 & NIBRSDescription == "Destruction, damage, vandalism")

# Create a bar chart of the distribution of "Simple assault" crimes
ggplot(filtered_data, aes(x = RMSOccurrenceHour)) +
  geom_bar(fill = "blue") +
  labs(title = "Distribution of Destruction, damage, vandalism Crimes in ZIP Code 77036",
       x = "Hour of Occurrence",
       y = "Count")

```

```{r}
# Load the necessary packages
library(dplyr)
library(ggplot2)

# Load your corrected dataset
data <- df2

# Filter the dataset to include only "Destruction, damage, vandalism" incidents in ZIP Code 77057
filtered_data <- data %>%
  filter(ZIPCode == 77057 & NIBRSDescription == "Destruction, damage, vandalism")

# Create a bar chart of the distribution of "Destruction, damage, vandalism" crimes
ggplot(filtered_data, aes(x = RMSOccurrenceHour)) +
  geom_bar(fill = "red") +
  labs(title = "Distribution of Destruction, damage, vandalism Crimes in ZIP Code 77057",
       x = "Hour of Occurrence",
       y = "Count")

```
```{r}
# Load the necessary packages
library(dplyr)
library(ggplot2)

# Assuming you've already loaded your dataset df2
data <- df2

# Filter the dataset to include only "Destruction, damage, vandalism" incidents in ZIP Code 77042
filtered_data <- data %>%
  filter(ZIPCode == 77042 & NIBRSDescription == "Destruction, damage, vandalism")

# Create a bar chart of the distribution of "Destruction, damage, vandalism" crimes
ggplot(filtered_data, aes(x = RMSOccurrenceHour)) +
  geom_bar(fill = "black") +
  labs(title = "Distribution of Destruction, damage, vandalism Crimes in ZIP Code 77042",
       x = "Hour of Occurrence",
       y = "Count")

```
```{r}
# Load the necessary packages
library(dplyr)
library(ggplot2)

# Assuming you've already loaded your dataset df2
data <- df2

# Filter the dataset to include only "Destruction, damage, vandalism" incidents in ZIP Code 77060
filtered_data <- data %>%
  filter(ZIPCode == 77060 & NIBRSDescription == "Destruction, damage, vandalism")

# Create a bar chart of the distribution of "Destruction, damage, vandalism" crimes in ZIP Code 77060
ggplot(filtered_data, aes(x = RMSOccurrenceHour)) +
  geom_bar(fill= "brown") +
  labs(title = "Distribution of Destruction, damage, vandalism Crimes in ZIP Code 77060",
       x = "Hour of Occurrence",
       y = "Count")

```
```{r}
# Load the necessary packages
library(dplyr)
library(ggplot2)

# Assuming you've already loaded your dataset df2
data <- df2

# Filter the dataset to include only "Destruction, damage, vandalism" incidents in ZIP Code 77072
filtered_data <- data %>%
  filter(ZIPCode == 77088 & NIBRSDescription == "Destruction, damage, vandalism")

# Create a bar chart of the distribution of "Destruction, damage, vandalism" crimes in ZIP Code 77072
ggplot(filtered_data, aes(x = RMSOccurrenceHour)) +
  geom_bar(fill= "darkgreen") +
  labs(title = "Distribution of Destruction, damage, vandalism in ZIP Code 77072",
       x = "Hour of Occurrence",
       y = "Count")

```
# Visualization
Here I will build graphs that will show the trends of the crimes in 2023 as well as the trends since 2019 and finally do some forecasting. 
Below is the total count of crime from January to August 
```{r}
Houstoncrime11 <- read_csv("Houstoncrime11.csv")
Houstoncrime11
```
## Visualization showing trend in 2023
This graph shows the evolution of crime for the first 8 months of 2023.
The graph shows a big drop in February 2023 (because period is not long it amplified the drop but it actually just a drop of about 10%) 
```{r}
# Create a data frame for the provided data
data <- data.frame(
  Months = c("Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug"),
  OffenseCount = c(21969, 20021, 22173, 22035, 22754, 21820, 22396, 20939)
)

# Convert the "Months" variable to a factor with proper ordering
data$Months <- factor(data$Months, levels = c("Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug"))

# Create a line graph to visualize the data
library(ggplot2)
ggplot(data, aes(x = Months, y = OffenseCount, group = 1)) +
  geom_line() +
  labs(title = "Monthly Offense Counts",
       x = "Month",
       y = "Offense Count")

```
Here I gathered more data related to crime related to crimes in Houston since 2019 and merge them with into a single file called houstoncrimebigdata
```{r}
houstoncrimebigdata<-  read_csv("houstoncrimebigdata.csv")
houstoncrimebigdata
```
I then finally plot the houstoncrimebigdata file to have a better understanding. As we can see it, number of crimes in 2023 are lower compared to 2022 but still higher than the 2019 numbers but at the same time population in Houston went from 6,245,000 to 6,707,000. The decrease is due mainly to the introduction of the One Safe Houston, a $44 million initiative launched in early 2022 to tackle crime when the city’s murder rate was on the rise. The plan included additional funds for crime prevention activities, overtime for police patrols, as well as programs to assist domestic violence survivors and individuals experiencing mental health crises [@Crime].
## Visualization showing general trend 
```{r}
# Load the necessary packages
library(ggplot2)

# Assuming you've imported the "houstoncrimedata" dataset
data <- read.table(text = "Date Count
Jan-19 17499
Feb-19 16195
Mar-19 18092
Apr-19 18688
May-19 20442
Jun-19 18943
Jul-19 20167
Aug-19 20203
Sep-19 18885
Oct-19 19676
Nov-19 18781
Dec-19 19520
Jan-20 22398
Feb-20 21206
Mar-20 20785
Apr-20 19300
May-20 22572
Jun-20 21564
Jul-20 20881
Aug-20 21988
Sep-20 20774
Oct-20 22404
Nov-20 21262
Dec-20 21699
Jan-21 21613
Feb-21 18356
Mar-21 21411
Apr-21 21261
May-21 22567
Jun-21 21875
Jul-21 21656
Aug-21 21748
Sep-21 21325
Oct-21 22179
Nov-21 20466
Dec-21 21148
Jan-22 21635
Feb-22 20187
Mar-22 22737
Apr-22 22298
May-22 23005
Jun-22 22009
Jul-22 22980
Aug-22 22828
Sep-22 21701
Oct-22 22269
Nov-22 19841
Dec-22 16765
Jan-23 21969
Feb-23 20021
Mar-23 22173
Apr-23 22035
May-23 22754
Jun-23 21820
Jul-23 22396
Aug-23 20939", header = TRUE)

# Create a line graph to visualize the data
ggplot(data, aes(x = as.Date(paste("01-", Date, sep = ""), format = "%d-%b-%y"), y = Count)) +
  geom_line() +
  scale_x_date(date_labels = "%b %y", date_breaks = "4 month") +
  labs(title = "Trend in Houston Crime Over Time",
       x = "Date",
       y = "Count")
```
## Visualization showing forecast for remaining of year 2023
From the number that I was able to get from my forecast, the number of crimes for yhe remaining of 2023 will almost stay at the same level of the previous months with a 5% fluctuation
```{r}
library(forecast)
crime_data <- data.frame(
  Date = as.Date(paste0("01-", houstoncrimebigdata$Date), format = "%d-%b-%y"),
  Count = houstoncrimebigdata$Count
)
crime_ts <- ts(crime_data$Count, frequency = 12, start = c(2019, 1))
plot(crime_ts, xlab = "Date", ylab = "Count", main = "Houston Crime Data")
fit <- ets(crime_ts)
forecasted_values <- forecast(fit, h = 4)
forecasted_values

```
