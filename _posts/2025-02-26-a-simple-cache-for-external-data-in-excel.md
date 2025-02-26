---
title: A simple cache for external data in Excel
tags: vba
---

For those of you working in the finance industry, you must have come across big spreadsheets feeding external data (e.g. Bloomberg) into pricing functions from internal libraries, full of volatile functions and slow to compute. Today I want to focus on how to store Bloomberg data in a very simple cache system to reduce the number of external calls and improve the recalculation speed of the worksheet.

At the top of the module, we create a `Dictionary` with a `Public` scope, which means that it can be accessed by any other function and it will retain data for as long as the spreadsheet is opened.

```vba
Public CURRENCIES = New Dictionary
```

Now we can wrap the call to Bloomberg API as per below:

```vba
Public Function GET_CURRENCY(byval ticker as string) as string

  If CURRENCIES.exists(ticker) Then
    items = CURRENCIES(ticker)

  Else
    items = Bloomberg.ReferenceData(ticker, "CRNCY")
    CURRENCIES.Add ticker, items
  End If

  GET_CURRENCY = items

End Function
```

Last, we can write a function to clear the cache without having to close the spreadsheet, and link it to a button for instance.

```vba
Public Function ResetCache()

  Set CURRENCIES = new Dictionary

End Function
```

Now the recalculation performance has dramatically improved!
