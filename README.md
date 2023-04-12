# semiAutomaticControlSelection
selects controls for given cases - e.g. non carriers vs carriers of a variant

```
setwd("~/Documents/R_Objects_DTU")
pheno<-read.table("BB_pheno.171subjects.txt",sep="\t",header=TRUE,row.names=1)
pheno$carrier<-1
pheno$carrier[rownames(pheno) %in% c("A077_00","A081_91","A154_04","A223_11","A319_14")] <-2
table(pheno$carrier)
head(pheno)
my_table<-pheno[,c("Status","Sex","AgeCat","carrier")]
```

divide into carriers / non carriers

```
data<-my_table

subset2 = data[data$carrier == 1, ]
subset1 = data[data$carrier == 2, ]
```

select a pool of controls to choose from
```
matched_controls = subset2[subset2$Status %in% subset1$Status &
                            subset2$AgeCat %in% subset1$AgeCat &
                            subset2$Sex %in% subset1$Sex, ]

dim(matched_controls)
head(matched_controls)
final_subset = matched_controls[matched_controls$carrier == 1, ]
```
```
dim(final_subset)
subset2
subset1
```
now select rows from carriers one by one and match tehm with controls

```
sel<-subset1[1,]
sel
matched_controls = subset2[subset2$Status %in% sel$Status &
                            subset2$AgeCat %in% sel$AgeCat &
                            subset2$Sex %in% sel$Sex, ]

dim(matched_controls)
matched_controls
#here I take the first 4 controls - for each case I'll take 4 controls
final<-matched_controls[1:4,]
```
repeat for each line
```
sel<-subset1[2,]
sel
matched_controls = subset2[subset2$Status %in% sel$Status &
                            subset2$AgeCat %in% sel$AgeCat &
                            subset2$Sex %in% sel$Sex, ]

matched_controls
final<-rbind(final,matched_controls[1:4,])
sel<-subset1[3,]
sel
matched_controls = subset2[subset2$Status %in% sel$Status &
                            subset2$AgeCat %in% sel$AgeCat &
                            subset2$Sex %in% sel$Sex, ]

matched_controls
final<-rbind(final,matched_controls[1:4,])
dim(final)
sel<-subset1[4,]
sel
sel<-subset1[5,]
sel
matched_controls = subset2[subset2$Status %in% sel$Status &
                            subset2$AgeCat %in% sel$AgeCat &
                            subset2$Sex %in% sel$Sex, ]

matched_controls
#here there were 8 and I took all 8 because rows 4 and 5 have the same matching criteria so I need 8 for these 2
final<-rbind(final,matched_controls)
dim(final)
controls<-final
controls
subset1
CFAP410_25<-rbind(subset1,controls)
dim(CFAP410_25)
head(CFAP410_25)

write.table(CFAP410_25,"CFAP410_25selected.txt",sep="\t",quote=FALSE)
```
