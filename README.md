
# Tourist and the impact of exchange rates
Is there a correlation between tourists in Switzerland and direct purchasing power between EUR/CHF and USD/CHF? 
Using data from the Federal Statistical Office (overnight stays) and publicly available exchange rates, I have set myself the goal of first visually processing this question and then statistically 

Eventough a high correlation was suspected, the data showed that there is none.
With a lag of 6 to 12 months, the p value did not improve. (autocorr was used and showed no suitable TS model)

## Key Take outs
- Tidyverse (perparing data)
- intesive GG Plot usage (data visualization)
- correlation with multiple features

## Structure of Folders
Folder: parent directory
r_bootcamp_-_01_hotelguests.Rmd               RMarkdown-file as Assignment
r_bootcamp_-_01_hotelguests.nb.html           HTML Output from hotelguests Markdown
r-bootcamp.Rproj                              Project File for R Studio

Folder: 01_input
data_01_hotels_angebot.xlsx	was downloaded from 
https://www.bfs.admin.ch/bfs/de/home/statistiken/kataloge-datenbanken/daten.html?dyn_prodima=901293 on 07. September 2020

data_02_logiernächte_ch.xlsx was downloaded from 
https://www.bfs.admin.ch/bfs/de/home/statistiken/kataloge-datenbanken/daten.html?dyn_prodima=901293 on 07. September 2020

data_04_eur-chf.xlsx and data_04_usd-chf.xlsx were created with help and queries from https://www.onvista.de/devisen/Euro-Schweizer-Franken-EUR-CHF

## EDA

Tidying with the tidyverse package:

```python
df_ln_inland_yearmon <-
  df_02_ln_ch %>% 
  select(jahre, contains("inland")) %>% 
  pivot_longer(cols = contains("inland"),
               names_to = "key",
               values_to = "anzahl_gaeste_inland") %>% 
  left_join(df_lookup_month_inland) %>% 
  mutate(year_mon = paste0(jahre, "-",month) %>% zoo::as.yearmon() %>% format("%Y-%m")) %>% 
  select(year_mon, anzahl_gaeste_inland)

df_ln_yearmon <- inner_join(df_ln_ausland_yearmon, df_ln_inland_yearmon, by = "year_mon")
  
rm(df_lookup_month_ausland, df_lookup_month_inland, df_ln_inland_yearmon, df_ln_ausland_yearmon)
```