# Introduction {#introduction}

The first chapter of the book largely focuses on providing background information on the project and exposition for the substance of the book. There are two figures displaying the location of US military personnel at two points in time (2005 and 2020). 

Note that the one thing I changed on this page is the base size for the font. In the book it's set to 30, but this is necessary to get the figures to render properly given the output size. Here I've set the base size to 12 to achieve a more appropriate scaling. This also seems to be sensitive to whether you're using a Mac or Windows, so just a heads-up that getting this to render properly may require some tweaks on your end depending on your operating system. 


```r
# Load libraries for itnroductory chapter
library(tidyverse)
library(tidyr)
library(cshapes)
library(maps)
library(countrycode)
library(scales)
library(here)
library(sf)
library(rnaturalearth)
library(rworldmap)
library(viridis)
library(sysfonts)
library(showtext)
library(knitr)

# Set resolution to 400
knitr::opts_chunk$set(comment = '', dpi = 400)

# Use custom font from Google fonts
sysfonts::font_add_google("Oswald", family = "oswald")
showtext::showtext_auto()

# Set base font size for custom theme
basesize <- 11 # Note this changed from book

# Set custom theme
theme_flynn_map <- function(){
  
  theme_void(base_family = "oswald", base_size = basesize) %+replace% 
    
    theme(plot.title = element_text(face = "bold", size = basesize, hjust = 0, margin = margin(t = 0, b = 0.3, l = 0, r = 0, unit = "cm")),
          plot.subtitle = element_text(size = basesize * 0.85, hjust = 0, margin = margin(t = 0, b = 0.3, l = 0, r = 0, unit = "cm")),
          plot.caption = element_text(face = "italic", size = basesize * 0.65, hjust = 1, margin = margin(t = 0.2, unit = "cm")),
          plot.background = element_rect(fill = "white", color = "white"),
          strip.background = element_rect(fill = "gray80", color = "black"),
          strip.text = element_text(color = "black", face = "bold"),
          panel.grid.major = element_line(color = "white", size = 0),
          panel.grid.minor = element_line(color = "white", size = 0),
          #axis.title = element_text(face = "bold", size = 0),
          #axis.title.y = element_text(margin = margin(t = 0, r = 0.5, b = 0, l = 0, unit = "cm")),
          #axis.title.x = element_text(margin = margin(t = 0.5, r = 0, b = 0, l = 0, unit = "cm")),
          legend.title = element_text(face = "bold", lineheight = 1.2),
          legend.position = "bottom",
          legend.key.height = unit(0.6, "cm"),
          legend.key.width = unit(2.5, "cm"))
}


# Set Seed
SEED <- 66502
set.seed(seed = SEED)
```




```r
# Use the troopdata function to obtain deployment data for 1950.
troop.data <- troopdata::get_troopdata(startyear = 1950, endyear = 1950) %>% 
  # Remove US from data
  filter(ccode != 2) %>% 
  # Change West Germany's code from 260 to 255
  mutate(ccode = ifelse(ccode == 260, 255, ccode))
```

```
Warning: Data include troop values for unknown locations and personnel listed as
'afloat'.
```

```r
# Use naturalearth package to create basemap.
map.base <- rnaturalearth::ne_countries(returnclass = "sf")

# Use cshapes package to generate COW system data for 1950
map.1950 <- cshp(date = as.Date("1950-01-01")) %>%  
  dplyr::mutate(., ccode = countrycode::countrycode(gwcode, "gwn", "cown")) %>% 
  left_join(troop.data, by = "ccode")
```

```
Warning in countrycode_convert(sourcevar = sourcevar, origin = origin, destination = dest, : Some values were not matched unambiguously: 711
```

```r
# Use ggplot and sf packages to create map of 1950 deployments
# Lay down base map first then deployments
# Note the application of the coordinate reference system below to alter projection from default
ggplot() +
  geom_sf(data = map.base, aes(geometry = geometry), fill = "gray90", color = "gray90", size = 0.1) +
  geom_sf(data = map.1950, aes(geometry = geometry, fill = troops), color = "white", size = 0.1) +
  theme_flynn_map() +
  theme(legend.text = element_text(size = basesize*1.05, margin = margin(t = -6, unit = "pt")),
        legend.title = element_text(size = basesize*1.1, lineheight = 0.3),
        plot.margin = margin(0, 0, 0, 0)) +
  viridis::scale_fill_viridis(option = "magma", direction = -1, begin = 0.1, end = 0.9, na.value = "gray90", breaks = c(0, 20, 200, 2000, 20000, 200000), limits = c(0, 200000), trans = "log1p", label = comma_format()) +
  coord_sf(crs = st_crs("ESRI:54030")) +
  labs(fill = "Deployment\nSize")
```

<img src="01-introduction_files/figure-html/maps of deployments in 1950-1.png" width="2800" />




```r
# As above, use troopdata package to get deployment data for 2020. Remove US, and change West Germany's country code.
troop.data <- troopdata::get_troopdata(startyear = 2020, endyear = 2020) %>% 
  filter(ccode != 2) %>% 
  mutate(ccode = ifelse(ccode == 260, 255, ccode))
```

```
Warning: Data include troop values for unknown locations and personnel listed as
'afloat'.
```

```r
# Use naturalearth package to create basemap.
map.base <- rnaturalearth::ne_countries(returnclass = "sf")

map.2020 <- cshapes::cshp(date = as.Date("2019-01-01")) %>%  
  dplyr::mutate(., ccode = countrycode::countrycode(gwcode, "gwn", "cown")) %>% 
  left_join(troop.data, by = "ccode")
```

```
Warning in countrycode_convert(sourcevar = sourcevar, origin = origin, destination = dest, : Some values were not matched unambiguously: 340, 816
```

```r
# Use ggplot and sf packages to create map of 1950 deployments
# Lay down base map first then deployments
# Note the application of the coordinate reference system below to alter projection from default
ggplot() +
  geom_sf(data = map.base, aes(geometry = geometry), fill = "gray90", color = "gray90", size = 0.1) +
  geom_sf(data = map.2020, aes(geometry = geometry, fill = troops), color = "white", size = 0.1) +
  theme_flynn_map() +
  theme(legend.text = element_text(size = basesize*1.05, margin = margin(t = -6, unit = "pt")),
        legend.title = element_text(size = basesize*1.1, lineheight = 0.3),
        plot.margin = margin(0, 0, 0, 0)) +
  viridis::scale_fill_viridis(option = "magma", direction = -1, begin = 0.1, end = 0.9, na.value = "gray90", breaks = c(0, 20, 200, 2000, 20000, 200000), limits = c(0, 200000), trans = "log1p", label = comma_format()) +
  coord_sf(crs = st_crs("ESRI:54030")) +
  labs(fill = "Deployment\nSize")
```

<img src="01-introduction_files/figure-html/maps of deployments in 2020-1.png" width="2800" />

