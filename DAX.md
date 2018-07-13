### power BI DAX

      Segment = IF(ABC_RFM_SCORE[R_SCORE]>4&& ABC_RFM_SCORE[R_SCORE]<=5&&
            ABC_RFM_SCORE[COMB_F_M_SCORE]>4 &&ABC_RFM_SCORE[COMB_F_M_SCORE]<=5,"Champions",
      
      IF(ABC_RFM_SCORE[R_SCORE]>2&& ABC_RFM_SCORE[R_SCORE]<=5&&
            ABC_RFM_SCORE[COMB_F_M_SCORE]>3 &&ABC_RFM_SCORE[COMB_F_M_SCORE]<=5,"Loyal Doctors",
      
      IF(ABC_RFM_SCORE[R_SCORE]>4&& ABC_RFM_SCORE[R_SCORE]<=5&&
            ABC_RFM_SCORE[COMB_F_M_SCORE]>0 &&ABC_RFM_SCORE[COMB_F_M_SCORE]<=1,"New Doctors",
      
      IF(ABC_RFM_SCORE[R_SCORE]>2&& ABC_RFM_SCORE[R_SCORE]<=3&&
            ABC_RFM_SCORE[COMB_F_M_SCORE]>2 &&ABC_RFM_SCORE[COMB_F_M_SCORE]<=3,"Doctors Needs Attention",
      
      IF(ABC_RFM_SCORE[R_SCORE]>2&& ABC_RFM_SCORE[R_SCORE]<=3&&
            ABC_RFM_SCORE[COMB_F_M_SCORE]>0 &&ABC_RFM_SCORE[COMB_F_M_SCORE]<=2,"About To Sleep",
      
      IF(ABC_RFM_SCORE[R_SCORE]>0&& ABC_RFM_SCORE[R_SCORE]<=2&&
            ABC_RFM_SCORE[COMB_F_M_SCORE]>2 &&ABC_RFM_SCORE[COMB_F_M_SCORE]<=5,"At Risk",
      
      IF(ABC_RFM_SCORE[R_SCORE]>0&& ABC_RFM_SCORE[R_SCORE]>=1&&
            ABC_RFM_SCORE[COMB_F_M_SCORE]>4 &&ABC_RFM_SCORE[COMB_F_M_SCORE]<=5,"Can't Loose Them",
      
      IF(ABC_RFM_SCORE[R_SCORE]<1&& ABC_RFM_SCORE[R_SCORE]>=2&&
            ABC_RFM_SCORE[COMB_F_M_SCORE]>1 &&ABC_RFM_SCORE[COMB_F_M_SCORE]<=2,"Hibernating",
      
      IF(ABC_RFM_SCORE[R_SCORE]>0&& ABC_RFM_SCORE[R_SCORE]<=2&&
            ABC_RFM_SCORE[COMB_F_M_SCORE]>0 &&ABC_RFM_SCORE[COMB_F_M_SCORE]<=2,"Lost",
      
      IF(ABC_RFM_SCORE[R_SCORE]>3&& ABC_RFM_SCORE[R_SCORE]<=4&&
            ABC_RFM_SCORE[COMB_F_M_SCORE]>0 &&ABC_RFM_SCORE[COMB_F_M_SCORE]<=1,"Promising",
      
      IF(ABC_RFM_SCORE[R_SCORE]>3&& ABC_RFM_SCORE[R_SCORE]<=5&&
            ABC_RFM_SCORE[COMB_F_M_SCORE]>1 &&ABC_RFM_SCORE[COMB_F_M_SCORE]<=3,"Potential Loyalist"
            
            )))))))))))


__Customer Segment      |     Activity    |     Actionable Tip    |     Recency Score Range     |     Frequency & Monetary Combined Score Range___

*     _Champions_         Bought recently, buy often and spend the most!        Reward them.Can be early adopters for new products. Will promote your brand.        4 to 5            4 to 5


*     _Loyal Customers_         Spend good money with us often. Responsive to promotions.         Upsell higher value products. Ask for reviews. Engage them.       2 to 5            3 to 5


*     _Potential Loyalist_            Recent customers, but spent a good amount and bought more than once.          Offer membership / loyalty program, recommend other products.           3 to 5            1 to 3


*     _Recent Customers_        Bought most recently, but not often.            Provide on-boarding support, give them early success, start building relationship.        4 to 5            0 to 1


*     _Promising_         Recent shoppers, but haven’t spent much.        Create brand awareness, offer free trials       3 to 4            0 to 1


*     _Customers Needing Attention_         Above average recency, frequency and monetary values. May not have bought very recently though.       Make limited time offers, Recommend based on past purchases. Reactivate them.       2 to 3            2 to 3


*     _About To Sleep_          Below average recency, frequency and monetary values. Will lose them if not reactivated.        Share valuable resources, recommend popular products / renewals at discount, reconnect with them.           2 to 3            0 to 2


*     _At Risk_           Spent big money and purchased often. But long time ago. Need to bring them back!          Send personalized emails to reconnect, offer renewals, provide helpful resources.         0 to 2            2 to 5


*     _Can’t Lose Them_        Made biggest purchases, and often. But haven’t returned for a long time.            Win them back via renewals or newer products, don’t lose them to competition, talk to them.           0 to 1            4 to 5


*     _Hibernating_       Last purchase was long back, low spenders and low number of orders.           Offer other relevant products and special discounts. Recreate brand value.          1 to 2            1 to 2

*     _Lost_            Lowest recency, frequency and monetary scores.        Revive interest with reach out campaign, ignore otherwise.        0 to 2            0 to 2


