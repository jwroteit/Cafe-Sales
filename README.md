# Cafe-Sales
generating insights from basic dataset on cafe sales data

Hello there!

I wanted to practice my data cleaning with a basic (dirty) dataset. I found Cafe-Sales on Kaggle and I've also included the original dataset in my file titled "original data." I wanted to look at cafe sales data specifcially because I was a barista for 4 years while I was in university and still to this day, coffee is a daily ritual for me.

More about the data: 8 columns, 10k rows which was composed of transaction IDs (unique idenitfier), date of transaction, item purchased, total transaction amount, payment method, and location

Tools used: Google Sheets ;)

Great, let's jump into my approach.

I wanted to see what data was available and started asking myself some questions before cleaning. I noticed:
- there is a unique identifier. I verified this by seeing if there were any duplicates.
- there is a transaction date but no time. Digging into this might be interesting.
- there are 8 different items sold. I wonder if one sells better than the other?
- there is a price per unit column. I wonder if this is consistent across items.
- there is a total transaction price. I wonder what the average transaction price is? 

After I got more familiar, I wanted to clean and standardize the data as much as possible. 
- There were null records as well as records with "ERROR" or "UNKNOWN" so I picked "Unknown" and filled in all the blank, "ERROR", and "UNKNOWN" records with "Unknown". This was applied to the follwing columns: Item, Payment Method, Location
- Next, I tackled the formatting in each column. I applied $Currency format to Price per Unit and Total Spent, Integer to Quantity, and Date_Time (YYYY-MM-DD) to Transaction Date.

After the data was cleaned, I wanted to get started on tackling some of those questions I had in the beginning. I also generated the following columns (explanations below): transation day of the week, transaction week of year, transaction month, estimated net profit per item, estimated net profit per transaction.
- Starting off with transaction date: through my experience as a barista, weekends were always way busier than the beginning of the week so I used the =WEEKDAY function to identify day of the week. I coupled this with an IFS statement to evaluate the output of =WEEKDAY and return the day of the week e.g. 1 = Sunday. I put this inside of an =IFERROR statement since I knew I had unknowns in there.
- I used the same logic for month of transaction date >> =MONTH inside an IFS statement, all inside an =IFERROR to address the "Unknowns"
- Lastly for transaction date, I wanted to look at week number as well which was more straightforward and only required =WEEKNUM inside a =IFERROR statement

I was satisified here intially so I started making some pivot tables. Let me save you the time, everything in this dataset was very evenly spread (at little too evenly if you ask me *laugh_cry*). 
- Every day of the week had roughly the same amount of orders, with Friday and Monday being the two highest.
- Similar findings with the month, October and March being the highest.
- The payment methods and location of the purchase were almost perfectly split up.
- The most common items bought was a little bit more fun to work with where Coffee and Salad both composed of over 13% of the total orders. It didn't start getting interesting until looking at Total Spent.
- Salad sales alone made up 22% of the total sales, followed by 16% for Sandwhich and Smoothie, around 12.5% for Juice and Cake, 9% for coffee, 6.5% for Tea and 4% for cookies.

This had me intrigued so I estimated profit margins for each item based on a quick Google Search (industry standard profit margins) to get an idea of no only what's driving sales but what's driving profit. Here are my assumptions about profit margins:
- Tea = 75% profit
- Juice = 25% profit
- Smoothie = 60% profit
- Cookie = 25% profit
- Cake  = 25% profit
- Salad = 20% profit
- Sandwich = 10% profit
- Coffee = 15% profit

I used an =IFS statement again to identify the food item, and apply the applicable profit margin to both the unit sold and the total transaction cost to return "Net Profit Per Unit Sold" and "Transaction Net Profit." Of course, also wrapped in an =IFERROR statement to take care of the unknowns.

Back to the pivot tables I go, and find some more interesting stuff. Based on my estimation of profit margins, Smoothies made up about 33%, Tea 16%, Salad 15%, Juice 11%, Sandwich and Tea 5%, and Cookies at 3%. Given that all 8 items sell at similar counts, but 3 of them are dirving 64% of the profit margin, they represent items that should be leveraged more. Notably, cookies are the least protifable and also produce the lowest sales.

Using this data to drive business decisions might look like:
- running a sale on smoothies
- pairing two high profit items e.g. buy a salad, get tea half off
- cake and coffee are marginally purchased for takeout more than in person, buy a cake, get coffee 25% off.
- removing cookies from the menu and replacing for a different pastry (maybe survey some customers to get more information on what they'd like to see in the shop)

Additional data that would be interesting and questions I would ask/aim to answer?
- Timestamps to idenitfy daily trends of higher sales volume. I would use this to determine if hours of operation are optimized for sales and for operating costs.
- Time spent in the cafe. This could be interesting because there is no clear pattern for in person or takeout. If the in-person people are staying for a long time, are they the folks driving the salad sales?
- Items bought together. This dataset only provides one item per transaction but knowing more details about the transaction might yield helpful results. For example, depending on the items commonly bought together, you can pair them better to drive sales. Another example of why this might be helpful, is to have specials. Are people buying a drink and food item together? only drinks? only food? Having more information here would be interesting.
- The payment type column made me think of discounts for paying cash given higher transaction fees from credit card companies. Given roughly 67% of transactions are either digital or by credit card, how does this translate into transaction fees and ultimately eat into profit?




