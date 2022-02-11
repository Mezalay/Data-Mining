library(tidyverse) library(ggplot2) library(data.table) library(dplyr)

##PartA

billboard = billboard %>% select(performer, song, year, week,
week\_position, song\_id, weeks\_on\_chart)

top\_songs = billboard %>% group\_by(performer, song)

yes = top\_songs %>% tally() %>% arrange(desc(n)) %>% data.frame() %>%
top\_n(10, wt=n)

colnames(yes)\[3\] &lt;- “count”

view(yes)

#As we can see, Radioactive by Imagine dragons is the most popular song
according to the billboard.

##PartB

div\_songs = billboard %>% group\_by(performer, song, year) %>%
filter\_all(all\_vars(.!=1958 & .!=2021))

view(div\_songs)

group\_by\_year &lt;- data.table(div\_songs)\[ , .(unique\_songs =
length(unique(song))), by = year\]

ggplot(group\_by\_year, aes(x=year)) + geom\_line(aes(y =
unique\_songs), color = “darkred”) + labs(x=“Year”, y=“number of unique
songs”, title=“Musical Diversity”, subtitle = “The number of unique
songs appeared on the Billboard by the year”, caption = “We can notice
how in the 60’s, the music diversity was increasing reaching 800 uniqe
songs in a year. However, the number started to fall sharply from the
late 60’s to the end of the century with almost half of the peak value
giving it sharp negative trend. Afterward, with the boom of the Pop
music in the US, the diversity surged again reaching almost 600 songs in
2010. Then it fall again by about 100 in magnitude by 2014, then bounced
back reaching the 800 level by 2020. This might be because of the rise
of the social media, and the strong music marketing industry recently.”)

#PartC

hit\_songs = billboard %>% select(weeks\_on\_chart, performer, song) %>%
group\_by(performer, song) %>% summarize(count = n()) %>%
arrange(desc(count)) %>% filter(count>=10)

view(hit\_songs)

singers = hit\_songs %>% group\_by(performer) %>% summarize(count = n())
%>% filter(count &gt;= 30) %>% arrange(desc(count))

view(singers)

ggplot(singers) + geom\_col(aes(fct\_reorder(performer, count), count))
+ coord\_flip(expand = TRUE) + labs(x=“Singers”, title = “10-Week Hit!”,
caption = “As we can see, there are 19 singers who were able to have at
least 30 songs for a 10-week hit on the Billboard. Feel sorry for
Imagine Dragons not to be in this list.”)
