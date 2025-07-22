# Customer-churn

## 🎯Project Overview
This project aims to analyze customer churn behavior and build a predictive model to identify customers at risk of leaving a service. The goal is to uncover key factors influencing churn and provide actionable insights for retention strategies.

## 📁 Dataset Description
The dataset contains information about customer demographics, services subscribed, account details, and whether the customer has churned. Key features include:
- Customer ID
- Gender, SeniorCitizen, Partner, Dependents
- Tenure, Contract, Payment Method
- MonthlyCharges, TotalCharges
- Churn (target variable)

## 🛠️ Tools Used
- **Language**: R
- **Libraries**: `tidyverse`, `ggplot2`, `dplyr`, `lubridate`
- **Reporting**: GitHub README (no R Markdown used)

## 🧹 Data Cleaning
### Steps performed:
    - Replaced the Senior Citizen column observrations.
    - Replaced the Paperless Billing column observations
    - Reorder the Churn status

## 📈 Key Analysis Steps
**1. Loading Packages:**
```r
  library(dplyr)
  library(ggplot2)
  library(scales)
  library(tidyverse)
```

**2. Importing the Data:**
```r
  setwd(folder/inpudata)
  customer_data <- read.csv("customer_data.csv")
```

**3. Data Cleaning:** standardized date formats.
  ```r
  # Replace the senior citizen & Paperless billing column Observations.
      customer_data <- customer_data %>% 
      mutate(SeniorCitizen = 
               case_when(SeniorCitizen ==0 ~ "Senior",
                SeniorCitizen ==1 ~ "Young_Middle_aged")) %>% 
      mutate(PaperlessBilling = 
               case_when(PaperlessBilling == "Yes" ~ "E-Bill",
                         PaperlessBilling == "No"~"Paper Bill"))

# Reorder the Churn status
      customer_data$Churn <- factor(customer_data$Churn, levels = c("Yes", "No"))      
```
 
**4. Exploratory Data Analysis:**
   - Customer Churn distribution
```r   
     ggplot(customer_data,aes(x=Churn),fill=Churn) +
        geom_bar(aes(fill=Churn),width = 0.3,position = "dodge") +
        scale_fill_manual(values = c("steelblue","orange")) +
        labs(title = "Customer Churn Distribution",
             x= "Customer Churn Status",
             y="Count of Customers")+
       theme_minimal(base_size = 14) +
        theme(legend.position = "none") 
```
   - Churn rate by Contract
```r  
     ggplot(customer_data, aes(x=Contract),fill = Churn)+
      geom_bar(aes(fill=Churn),stat = "count",width = 0.5) +
      scale_fill_viridis_d(option = "D") +
      labs(title = "Churn rate by Contract",
           x="Contract type",
           y="Count of Customers") +
      theme_minimal(base_size = 14)

```
   - Churn by Tenure
```r
     ggplot(customer_data,aes(x=tenure,fill = Churn))+
      geom_histogram(binwidth = 5,position = "fill") +
      scale_fill_viridis_d(option = "D") +
      labs(title = "Churn rate by tenure",
           x= "Tenure",
           y="Proposition") +
      theme_minimal(base_size = 14)
```
   - Monthly Charges vs Churn
```r
     ggplot(customer_data,aes(x= MonthlyCharges,fill = Churn)) +
      geom_density(alpha =0.5,stat= "density") +
      labs(title = "Monthly Charges Distribution by Churn",
           y="Density") +
      theme_minimal(base_size = 14)
```
   - Total Charges by Churn
```r
     ggplot(customer_data,aes(x=Churn,y=TotalCharges)) +
      geom_violin(aes(fill=Churn),stat = "ydensity",alpha=0.5) +
      geom_boxplot(fill = NA,outliers = F,width=0.1,size = 0.5) +
      labs(title = "Total charges by Churn",
           x="Customer Churn Status",
           y="Total Charges") +
      scale_fill_viridis_d(option = "D") +
      theme_minimal(base_size = 14) +
      theme(legend.position = "none")
```
   - Churn Rate by Internet Service
```r
     ggplot(customer_data, aes(x = InternetService, fill = Churn)) +
      geom_bar(position = "fill",width = 0.5) +
      labs(title = "Churn Rate by Internet Service",
           y="Proposition") +
      scale_fill_viridis_d(option = "D") +
      theme_minimal(base_size = 14) 
```
  - Churn rate vs Type of Payment
```r
     payment_type <- customer_data %>% 
      group_by(PaymentMethod) %>% 
      summarise(count_customers = n())

      ggplot(payment_type,aes(x=PaymentMethod,y=count_customers,fill=PaymentMethod,color=PaymentMethod,))+
        geom_segment(aes(x = PaymentMethod,xend = PaymentMethod,y=0,yend=count_customers),size = 2) +
        geom_point(size=10,shape=21) +
        scale_color_viridis_d() +
        scale_fill_viridis_d() +
        labs(title = "Churn rate based on Payment type",y="Count of Customers") +
        theme_bw() +
        theme(
          plot.title = element_text(face = "bold", size = 14),
          axis.title.x = element_text(face = "bold", size = 12),
          axis.title.y = element_text(face = "bold", size = 12),
          legend.position = "none") 
```
   - Churn rate by Billing Type
```r
      ggplot(customer_data,aes(x=Churn,fill=Churn))+
        geom_bar(width=0.5)+
        scale_fill_manual(values = c("olivedrab3","steelblue")) +
        labs(title = "Churn rate by billing type",
             x="Customer Churn status",
             y="Count of Customers") +
        theme_bw() +
        theme(
          plot.title = element_text(face = "bold", size = 14),
          axis.title.x = element_text(face = "bold", size = 12),
          axis.title.y = element_text(face = "bold", size = 12),
          legend.position = "none")  +
        facet_grid(~PaperlessBilling)
```

# 💡 Insights & Recommendations
  
  - Customers with month-to-month contracts are more likely to churn.
  - New customers (tenure < 12 months) are at higher risk.
  - Electronic check users show higher churn rates.
  - High monthly charges may be a churn driver.
  - Consider offering incentives for long-term contracts.

# 📈 Visualization with Insights
   
  **Churn distribution across Customers**

    ![Customer Churn rate distribution](https://github.com/Saravanan-dataworks/Customer-Churn-Analysis/blob/main/Customer%20Churn%20rate%20distribution.png)

  **Contract Type Strongly Influences Churn**

   - Customers with month-to-month contracts have a significantly higher churn rate compared to those on one-year or two-year contracts. 
    This suggests that longer-term contracts may help reduce churn.
   - Promote vouchers for longer-term contracts.

    ![Churn rate by Contract](https://github.com/Saravanan-dataworks/Customer-Churn-Analysis/blob/main/Churn%20rate%20by%20Contract.png)

  **Tenure is a Key Indicator**
  
   - Customers who have been with the company for a shorter period (0–12 months) are more likely to churn.
   - Retention efforts should focus on new customers during their first year.

     ![Churn rate by tenure](https://github.com/Saravanan-dataworks/Customer-Churn-Analysis/blob/main/Churn%20rate%20by%20tenure.png)

  **Electronic Check Users Are More Likely to Churn**
   
   - Customers who pay via electronic check show a higher churn rate than those using credit cards or bank transfers.
   - This could indicate a correlation between payment method and customer satisfaction or stability.

     ![Churn rate based on Payment type](https://github.com/Saravanan-dataworks/Customer-Churn-Analysis/blob/main/Churn%20rate%20based%20on%20Payment%20type.png)

  **High Monthly Charges Correlate with Churn**

   - Customers with higher monthly charges tend to churn more.
   - This could be due to perceived value or affordability issues..

     ![Monthly Charges distribution by Churn.png](https://github.com/Saravanan-dataworks/Customer-Churn-Analysis/blob/main/Monthly%20Charges%20distribution%20by%20Churn.png)

  **Paperless Billing Preference**

   - Customers who opt for paperless billing may be more tech-savvy but also more likely to switch providers if dissatisfied.
   - This group may respond well to digital engagement strategies.

     ![Churn rate by billing type](https://github.com/Saravanan-dataworks/Customer-Churn-Analysis/blob/main/Churn%20rate%20by%20billing%20type.png)

  **Internet Service Type Matters**

    - Customers using fiber optic or DSL services churn more than those without internet service. 
    - This could point to service quality or pricing concerns.

    ![Churn rate by Internet Service](https://github.com/Saravanan-dataworks/Customer-Churn-Analysis/blob/main/Churn%20rate%20by%20Internet%20Service.png)

# Recommendations & Campaign ideas:
  **1. Target High-Risk Segments**
        Insight: Customers with month-to-month contracts show significantly higher churn rates.
        
        Action:
        
        - Introduce loyalty programs or discounts for customers who switch to longer-term contracts.
        - Offer personalized retention incentives (e.g., free upgrades or reduced fees) for month-to-month customers.
        
  **2. Improve Customer Experience for Low-Tenure Users**
        
        Insight: Churn is higher among customers with short tenure (e.g., <12 months).
        
        Action:
        
        - Implement onboarding programs to engage new customers early.
        - Use proactive customer support (e.g., check-ins, satisfaction surveys) during the first few months.
        
  **3. Review Pricing Strategy**
  
        Insight: Customers with higher monthly charges tend to churn more.
        
        Action:
        
        - Re-evaluate pricing tiers and ensure value alignment.
        - Offer bundled services or discounts for high-paying customers to increase perceived value.
        
  **4. Address Internet Service Issues**
        Insight: Churn varies significantly by Internet service type.
        
        Action:
        
        - Investigate service quality issues (e.g., speed, reliability) for high-churn service types.
        - Provide service upgrades or satisfaction guarantees for affected customers.
  **5. Optimize Payment Methods**
        Insight: Certain payment methods are associated with higher churn.
        
        Action:
        
        - Encourage auto-pay or credit card payments through incentives (e.g., cashback, discounts).
        - Simplify payment processes and improve transparency in billing.
        
  **6. Promote Paperless Billing**
        Insight: Customers using paperless billing may show different churn patterns.
        
        Action:
        
        - Promote paperless billing with benefits like faster processing and eco-friendly messaging.
        - Ensure digital billing platforms are user-friendly and reliable.
        
## 📂 Folder Overview
- `scripts/` → Customer churn Analysis.txt


## 🚀 How to Run
Follow these steps to set up and run the Bellabeat Smart Watch User Analysis project on your local machine.

**1. Download the code**:

Download the from attached files. 

**2. Open the Project in RStudio**:

Open the .Rproj file (if available) or open the folder in RStudio.
     
**3. Install Required Packages**:

Make sure you have the necessary R packages installed:
   - install.packages(c("tidyverse", "lubridate", "ggplot2", "dplyr", "readr", "ggplot2"))

**4. Set the Working Directory**

Set your working directory to the project folder:
  - setwd("path/to/your/cloned/folder")
   
  

