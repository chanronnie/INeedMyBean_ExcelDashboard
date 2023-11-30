
![CoffeeSales](https://github.com/chanronnie/INeedMyBeans_ExcelDashboard/assets/121308347/fbcf7d38-9d9c-4ed9-87a2-5a0ed49f6c53)


## About this project

**Objective**:<br>
The goal of this project is to create a dashboard that displays the overall sales figures and performance of the fictional coffee shop *I Need My Beans!* to the owners.
The end-users (the owners) should be able to identify:
- the monthly sales and transaction trends
- the peak sales period
- the best-selling menu items by orders


**Data Source**:<br>
The data used is fictional and sourced from *Maven Analytics*. It contains order details of all transactions recorded from January to June 2023.

## Tool Used
![Google Sheets](https://img.shields.io/badge/Google_Sheets-217346?style=for-the-badge&logo=google-sheets&logoColor=white)


## File Contents
- [CoffeeSales_RAW.xlsx](CoffeeSales_RAW.xlsx):This file contains the raw dataset.
- [CoffeeSales_WORKBOOK.xlsx](CoffeeSales_WORKBOOK.xlsx): This worbook includes my data analysis along with the dashboard.

## Approach
The initial step involves performing data cleaning on the dataset. Surprisingly, there are no duplicated transaction records in the dataset. The next step is to split the `transaction_date` column into `Month`, `Day of Week`, and `Hour`. Additionally, I separated the `product_detail` column into `item` and `size` (Sm, Rg, Lg) using the following formula (including whitespace trimming):

```EXCEL
=IF(OR(RIGHT(N2,2)="Sm",RIGHT(N2,2)="Rg",RIGHT(N2,2)="Lg"),RIGHT(N2,2)," ")
```
This helps reducing data redundancy, thereby generating a more representative bar chart.

1. **For the KPIs**, I chose to display the total sales, total transactions, and the number of coffees sold, along with their corresponding charts illustrating monthly trends from January to June 2023.

2. **To represent the sales breakdown** by the hour of the day and by days of the week, I used bar charts to track sales, highlighting the peak sales period with a darker color.

3. **For the heatmap**, I created a separate worksheet to allow uniform resizing of columns and rows into narrower squares. By utilizing `SUMIFS`, conditional formatting, and number format customization, I successfully built a heatmap for the sales breakdown across both `hours` and `days of the week`. *(Note that heatmap chart is not available in Google Sheets)*

4. **To compare categories**, I used a horizontal bar chart and arranged the categories in descending order. Once again, the bar/category with the highest quantity sold has been colored in a darker tone.

5. **For displaying items within the selected category**, I opted for the **`QUERY`** function instead of Pivot Tables. This function is more flexible and performs better. With just one formula, I can display three pieces of information (`Items`, `Quantity`, and `Sales`) for the selected category:

```EXCEL
=QUERY(
      WorkingSheet!13:149129,                                -- dataset
      "select                                                    
            O,                                               -- "item" column
            SUM(G),                                          -- sum of the "transaction_qty" column
            SUM(Q)                                           -- sum of the "sales" column
        where L = '"&J12&"'                                  -- where "category" is the category selected by the user
        group by O                                           
        order by SUM(G) DESC                                 -- sort the output by total quantity in descending order
        label O 'Items', SUM(G) 'Quantity', SUM(Q) 'Sales'   -- rename columns
      "
)
```

Here is a preview of the query output for the selected category **Branded**.

Items	| Quantity | Sales
--- | --- | ---
I Need My Bean! Latte cup | 315 | $4,509
I Need My Bean! Diner mug | 240 | $2,935
I Need My Bean! T-shirt | 221 | $6,163


## Key Findings
- Sales and Transactions exhibit a positive trend as the months progress.
- The morning rush on Mondays, Thursdays, and Fridays marks the peak sales period.
- Weekends experience significantly lower sales compared to weekdays.
- The most popular orders include coffees, teas, and bakery items, with 42% of orders placed for coffees.
- Ethiopian and Brazilian coffees are the most popular flavors, while Jamaican Coffee River generates more sales.
- Packaged Chocolate is the least ordered item.


## Recommendations
- Focus on staffing and stocking during peak sales periods: Morning rush (8 am - 10 am) on Mondays, Thursdays, and Fridays.
- Consider adjusting business hours, considering the drastic decrease in sales after 7 pm and on weekends. Suggested hours are 7 am to 7 pm on weekdays and reduced hours on weekends.
- Remove Packaged Chocolate items from the menu.





## Note
To ensure compatibility and an accurate representation of the dashboard, it is strongly advised to open the workbook in Google Sheets.
