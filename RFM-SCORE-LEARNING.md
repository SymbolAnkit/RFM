   __Clear environment__

    rm(list = ls())

   __set working directory__
   
    setwd("C:/Users/Ankit/Desktop/RFM")

   ### load required libraries

    library(RODBC)
    library(plyr)
    library(dplyr)
    library(stringr)
    library(readxl)

   #### Make connection with sql server

    dbhandle <- odbcDriverConnect('Driver={SQL Server};server=SN;database=DBN;trusted_connection=True')

 __Extract data from sql server__

    res <- sqlQuery(dbhandle,"SELECT * FROM table_Name")

__extract date from date - time__

    res$Date <- substr(res$TransactionDate,1,10)

    res$Date <- as.Date(res$Date , "%Y-%m-%d")

    res$RefDocCode <- as.character(res$RefDocCode)

    sum(is.na(res$HomeCollectionCharges))

    summary(res$HomeCollectionCharges)

    res$HomeCollectionCharges[which(is.na(res$HomeCollectionCharges))] <- 0

    res <- res[order(res$Date),]

   *__calulate last purchase days__*

     res$last.ref.days <- as.numeric(difftime(Sys.Date(),res$Date , units = "days"))

__work start - find least frequent one__

         drdata <- res %>% group_by(RefDocCode) %>% 
         summarise(netvalue = sum(HomeCollectionCharges),
                                    freq = length(Date),
                              recency = min(last.ref.days))

         drdata10 <- filter(drdata , drdata$freq < 6)

         least_ref_doctor <- drdata10$RefDocCode

      use_data <- res[ ! res$RefDocCode %in% least_ref_doctor ,  ]

__work end__

   ###### group by 

      final <- use_data %>% group_by(RefDocCode) %>% 
                summarise(R_VALUE = min(last.ref.days),
                                F_VALUE = length(Date),
                   M_VALUE = sum(HomeCollectionCharges) )


         final <- final[ ! final$M_VALUE <= 50 ,]

      summary(final$R_VALUE)
      summary(final$F_VALUE)
      summary(final$M_VALUE)

__mark cuts__

      Recency_cuts <- quantile(final$R_VALUE , probs = seq(0.20 ,0.80 , by=0.20))
      Frequency_cuts <- quantile(final$F_VALUE , probs = seq(0.20,0.80, by=0.20))
      Monetary_cuts <- quantile(final$M_VALUE , probs = seq(0.20,0.80,by=0.20))

__get score corresponding to cuts__

      final$R_SCORE <- findInterval(final$R_VALUE , c(-Inf,Recency_cuts,Inf))
      final$F_SCORE <- findInterval(final$F_VALUE , c(-Inf,Frequency_cuts,Inf))
      final$M_SCORE <- findInterval(final$M_VALUE , c(-Inf,Monetary_cuts,Inf))

      final$COMB_F_M_SCORE <- ((final$F_SCORE + final$M_SCORE)/2)

__calculate RFM score__

      final$RFM_SCORE <- paste(final$R_SCORE,final$F_SCORE,final$M_SCORE,sep = "")

__Mapping__

      refrnce_doc <- sqlQuery(dbhandle,"SELECT column names FROM
                        table name2 ")
 
      final$RefDocCode <- trimws(final$RefDocCode)

##### remove white spaces

      refrnce_doc$RefDocCode <- trimws(refrnce_doc$RefDocCode)
      refrnce_doc$DocName <- trimws(refrnce_doc$DocName)
      refrnce_doc$DocCity <- trimws(refrnce_doc$DocCity)

   ###### convert to upper case

      refrnce_doc$DocCity <- toupper(refrnce_doc$DocCity)
      refrnce_doc$DocName <- toupper(refrnce_doc$DocName)

###### remove punctuations

      refrnce_doc$DocCity <- gsub("[[:punct:][:blank:]]+", " ", refrnce_doc$DocCity)
      refrnce_doc$DocName <- gsub("[[:punct:][:blank:]]+", " ", refrnce_doc$DocName)

###### unique values

      doc <- unique(subset(refrnce_doc,select = c("RefDocCode","DocName","DocCity")))

###### merge the data

      target <- merge(final,doc,by.x = "RefDocCode",by.y = "RefDocCode",all.x = TRUE)

      tosql <- data.frame(target)

###### save to sql server 
   
        sqlSave(dbhandle,tosql,tablename = "DF_NAME",rownames = F,colnames = F,safer = F,fast = T)

__save to csv file__
         
         write.csv(target,"RFM_DOCTOR_GR.csv" , row.names = F)

