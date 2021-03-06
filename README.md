Flow Exploratory analysis
================
Joy Lee-Shi
05/03/2022

## Plots

``` r
flow <- read.csv("merged.csv", fileEncoding="UTF-8-BOM")

flow1 <- flow[,c("mean_CSB1", "mean_AAM1", "mean_CG1", "mean_UAF1", "mean_TC1", 
                 "mean_SOC1", "mean_LSC1", "mean_TT1", "mean_AE1", "Genre",
                 "total_DFS")]

flow1$Genre <- as.factor(flow1$Genre)
levels(flow1$Genre)[levels(flow1$Genre)=="1"] <- "Classical"
levels(flow1$Genre)[levels(flow1$Genre)=="2"] <- "Jazz"

flow2 <-  flow[,c("mean_CSB22", "mean_AAM22", "mean_CG22", "mean_UAF22", "mean_TC22", 
                 "mean_SOC22", "mean_LSC22", "mean_TT22", "mean_AE22", "Genre",
                 "total_FSS")]

flow2$Genre <- as.factor(flow2$Genre)
levels(flow2$Genre)[levels(flow2$Genre)=="1"] <- "Classical"
levels(flow2$Genre)[levels(flow2$Genre)=="2"] <- "Jazz"
```

``` r
# Density plots with semi-transparent fill

traitDplot <- 
  ggplot(flow1, aes(x = total_DFS, color = Genre)) +
      geom_density() +
      scale_color_manual(values=wes_palette("Darjeeling1", n = 2)) +
      labs(
        x = "Trait Flow Scores",
        y = "Density") +
      xlim(19,50) +
      ylim(0, .11) +
      theme(text = element_text(size = 25)) +
  theme(legend.position="top")

stateDplot <- 
  ggplot(flow2, aes(x = total_FSS, color = Genre)) +
      geom_density() +
      scale_color_manual(values=wes_palette("Darjeeling1", n = 2)) +
      labs(
        x = "State Flow Scores",
        y = "Density") +
      xlim(19,50) +
      ylim(0, .11) +
   theme(text = element_text(size = 25)) +
      theme(legend.position = "none")

traitDplot/ stateDplot
```

![](state_trait_dist.png)<!-- -->

``` r
ggsave("state_trait_dist.png", plot=traitDplot+stateDplot, width=10, height=6)

Years <-
  ggplot(flow, aes(x = Genre, y = YearsPlayed)) +
  geom_jitter(aes(color = Genre, shape = Genre, size=5), position = position_jitter(0.1)) +
  theme(legend.position = "none") +
  theme(text = element_text(size = 15)) +
   scale_color_manual(values=alpha(wes_palette("Royal1", n = 2), .5))

HoursWeek <-
  ggplot(flow, aes(x = Genre, y = HoursPractedWeek)) +
  geom_jitter(aes(color = Genre, shape = Genre, size=5), position = position_jitter(0.1)) +
  theme(legend.position = "none") +
  theme(text = element_text(size = 15)) +
   scale_color_manual(values=alpha(wes_palette("Royal1", n = 2), .5))

LastPr <-
    ggplot(flow, aes(x = Genre, y = LastPracticed)) +
  geom_jitter(aes(color = Genre, shape = Genre, size=5), position = position_jitter(0.1)) +
  theme(legend.position = "none") +
  theme(text = element_text(size = 15)) +
   scale_color_manual(values=alpha(wes_palette("Royal1", n = 2), .5))


AgeStart <-
  ggplot(flow, aes(x = Genre, y = AgeStarted)) +
  geom_jitter(aes(color = Genre, shape = Genre, size=5), position = position_jitter(0.1)) +
  theme(legend.position = "none") +
  theme(text = element_text(size = 15)) +
   scale_color_manual(values=alpha(wes_palette("Royal1", n = 2), .5))

ggsave("HoursWeek.png", plot=HoursWeek+LastPr, width=6, height=6)
ggsave("AgeStart.png", plot=AgeStart+Years, width=6, height=6)
```
![](HoursWeek.png)<!-- -->

![](AgeStarted.png)<!-- -->

``` r
data1 <- flow[,c("Genre", "mean_CSB1", "mean_AAM1", "mean_CG1", "mean_UAF1", "mean_TC1", 
                 "mean_SOC1", "mean_LSC1", "mean_TT1", "mean_AE1")]
data1 <- cbind(data1[1], stack(data1[2:10]))
DFS <- ggplot(data1, aes(x=ind, y=values, fill=data1$`Genre`)) + 
    geom_boxplot() +
    geom_jitter(width=0.1,alpha=0.2) +
    scale_fill_manual(values = alpha(wes_palette("Royal1", n = 2), .4)) +
  theme(text = element_text(size = 20)) +
    scale_y_continuous(name = "DFS Average Scores") +  # Continuous variable label
    scale_x_discrete(name = "Dimensions") +
    facet_wrap(~data1$Genre) +
    coord_flip()

data2 <-  flow[,c("Genre", "mean_CSB22", "mean_AAM22", "mean_CG22", "mean_UAF22", "mean_TC22", 
                 "mean_SOC22", "mean_LSC22", "mean_TT22", "mean_AE22")]
data2 <- cbind(data2[1], stack(data2[2:10]))
FSS <- ggplot(data2, aes(x=ind, y=values, fill=data2$`Genre`)) + 
    geom_boxplot() +
    geom_jitter(width=0.1,alpha=0.2) +
    scale_fill_manual(values = alpha(wes_palette("Royal1", n = 2), .4)) +
    scale_y_continuous(name = "FSS Average Scores") +  # Continuous variable label
    scale_x_discrete(name = "Dimensions") +
  theme(text = element_text(size = 20)) +
    facet_wrap(~data2$Genre) +
    coord_flip()
DFS
```

![](DFS.png)<!-- -->

``` r
FSS
```

![](FSS.png)<!-- -->

``` r
ggsave("DFS.png", plot=DFS, width=10, height=4)
ggsave("FSS.png", plot=FSS, width=10, height=4)
```

``` r
a <- flow[,c("Genre", "mean_CSB1", "mean_AAM1", "mean_CG1", "mean_UAF1", "mean_TC1", 
                 "mean_SOC1", "mean_LSC1", "mean_TT1", "mean_AE1")]
names(a)[names(a)=="mean_CSB1"] <- "ChallengeSkill Balance"
names(a)[names(a)=="mean_AAM1"] <- "ActionAwareness Merging"
names(a)[names(a)=="mean_CG1"] <- "Clear Goals"
names(a)[names(a)=="mean_UAF1"] <- "Unambiguous Feedback"
names(a)[names(a)=="mean_TC1"] <- "Total Concentration"
names(a)[names(a)=="mean_SOC1"] <- "Sense of Control"
names(a)[names(a)=="mean_LSC1"] <- "Loss of SelfConsciousness"
names(a)[names(a)=="mean_TT1"] <- "Time Transformation"
names(a)[names(a)=="mean_AE1"] <- "Autotelic Experience"
a$Genre <- as.factor(a$Genre)
levels(a$Genre)[levels(a$Genre)=="Classical"] <- "ClassicalDFS"
levels(a$Genre)[levels(a$Genre)=="Jazz"] <- "JazzDFS"

b <-  flow[,c("Genre", "mean_CSB22", "mean_AAM22", "mean_CG22", "mean_UAF22", "mean_TC22", 
                 "mean_SOC22", "mean_LSC22", "mean_TT22", "mean_AE22")]
names(b)[names(b)=="mean_CSB22"] <- "ChallengeSkill Balance"
names(b)[names(b)=="mean_AAM22"] <- "ActionAwareness Merging"
names(b)[names(b)=="mean_CG22"] <- "Clear Goals"
names(b)[names(b)=="mean_UAF22"] <- "Unambiguous Feedback"
names(b)[names(b)=="mean_TC22"] <- "Total Concentration"
names(b)[names(b)=="mean_SOC22"] <- "Sense of Control"
names(b)[names(b)=="mean_LSC22"] <- "Loss of SelfConsciousness"
names(b)[names(b)=="mean_TT22"] <- "Time Transformation"
names(b)[names(b)=="mean_AE22"] <- "Autotelic Experience"
b$Genre <- as.factor(b$Genre)
levels(b$Genre)[levels(b$Genre)=="Classical"] <- "ClassicalFSS"
levels(b$Genre)[levels(b$Genre)=="Jazz"] <- "JazzFSS"

a <- cbind(a[1], stack(a[2:10]))
b <- cbind(b[1], stack(b[2:10]))
c <- rbind(a,b)

library(gghalves)
c %>% 
  ggplot(aes(x= c$Genre, y=values, fill=c$Genre)) +
  geom_half_boxplot(nudge=.05) +
  geom_half_violin(aes(fill = c$Genre),
                   side = "r", nudge = 0.01) +
  scale_fill_manual(values = alpha(wes_palette("Royal1", n = 4), .6)) +
  xlab("Group")+ 
  facet_wrap(~ind) +
  theme(legend.position = "bottom") +
  theme(text = element_text(size = 24)) +
  theme(axis.title.x=element_blank(),
        axis.text.x=element_blank(),
        axis.ticks.x=element_blank()) +
  labs(y = "Average Dimension Scores") +
  guides(fill = guide_legend(nrow = 1)) -> cc

cc
```

![](plot1.png)<!-- -->

``` r
ggsave("plot1.png", plot=cc, width=10, height=12)
```
