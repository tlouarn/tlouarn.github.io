---
title: A Simple Cache System for External Data in Excel
tags: excel vba
---

If you’re in the finance industry, chances are you’ve encountered massive Excel workbooks that rely on external data sources—such as Bloomberg—to feed into complex pricing models. These spreadsheets are often full of volatile functions, causing slow recalculation times that can be a real productivity killer. In this post, I’ll walk you through how to set up a simple cache system for Bloomberg data in Excel, reducing the number of external calls and dramatically improving the performance of your worksheet.

# Step 1: Creating a Persistent Cache

At the core of our caching solution is a `Dictionary` object that stores the data temporarily. By defining it with Public scope, the cache will remain accessible across all functions within the spreadsheet as long as the workbook is open.

```vb
Public CURRENCIES = New Dictionary
```
This `Dictionary` will hold our external data (such as Bloomberg quotes) and prevent redundant API calls, speeding up the recalculation process.

# Step 2: Wrapping the Bloomberg API Call

Next, we’ll create a function that checks the cache first before making a call to Bloomberg. If the data is already in the cache, we’ll return it immediately. If not, we’ll fetch it from Bloomberg and store it in the cache for future use.


```vb
Public Function GET_CURRENCY(ByVal ticker As String) As String
    Dim items As String

    ' Check if the data is already in the cache
    If CURRENCIES.Exists(ticker) Then
        items = CURRENCIES(ticker)  ' Retrieve data from the cache
    Else
        ' If not, fetch from Bloomberg and cache it
        items = Bloomberg.ReferenceData(ticker, "CRNCY")
        CURRENCIES.Add ticker, items
    End If

    GET_CURRENCY = items  ' Return the data
End Function
```

With this function, any calls to `GET_CURRENCY` will first check if the data for the given ticker is already available in the cache. If it is, the function returns the cached data; otherwise, it fetches the data from Bloomberg and adds it to the cache.

# Step 3: Clearing the Cache
Finally, to keep the cache fresh or to troubleshoot, you might want to clear the cached data without closing the spreadsheet. Here’s a simple function to reset the cache:

```vb
Public Function ResetCache()
    Set CURRENCIES = New Dictionary  ' Clear the cache
End Function
```

You can link this function to a button in your spreadsheet for easy access. Clicking this button will clear the cache, forcing Excel to fetch fresh data from Bloomberg the next time a request is made.
Conclusion: Faster Recalculations

By using this simple cache system, you can significantly improve the recalculation performance of your Excel workbooks. The caching mechanism reduces the number of calls to external data sources, which is especially helpful in complex financial models that require frequent updates.

Try it out, and you’ll notice how much faster your worksheets respond, especially when dealing with large sets of external data.
