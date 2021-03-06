# Interview Question 

In this scenario your CFO has noticed that the expense reports being submitted by their employees looks off. He has tasked you with developing a report that will track certain information regarding  employee's expense reports. 

## Report Information 

### Steps

1. Determine by employee type which people have expensed more than the allocated amount for that type, which in this case will be Developer and Manager
2. Although there is some leeway with forgetting to submit expenses, determine which employees by employee type have forgotten to report more than their allowed amount based on the department.  
3. Determine by employee type who is spending more on average than their group for a given period.
4. Determine any suspicious employees, that is in any given timeframe their total expenses is greater than 1 standard deviation than the average for that group for that same timeframe.

### Additional Information
He wishes for this to be a report for a screen on the company internal website. Therefore this will need  to be in the form of a stored procedure. Moreover the CFO wishes to be able to view this information  in three different formats. That is: 

1. All Time. That is using all of the data we currently have to determine the information above 
2. Monthly. That is using all the data within a specific month to determine the data 
3. Date Range. That is using the data between a specific 5 days to determine the </br> information above

There should also be an output containing errors based on the mode above. For example if 
 someone passes 7 days into the Date Range mode, a month not contained in the table, etc. 

# Notes 
- Missing Expenses will be denoted as NULL in the expense column in the ExpenseInfoTable

- For calculating steps 1, 3, 4 above, you are assume that any missing expense will be the average  for that time period for that employee. 

- The data for this example will be truncated between a 3 month period between 01/01/2021 and 03/30/2021. However it should still work if we were to add more data into the table for a different month 

- You can assume that the parameters for the stored procedure will be @Mode,  @TimeFrameStart, and @TimeFrameEnd 

- The DateRange mode should only work for a 5 day period.

- You will only need to calculate data on either a 5 day period, monthly period, or all time period. For all time period you can assume that the total amount they can expense will be the OneMonthExpenseMax, denoted in the DepartmentMetaData table,  multiplied by the number of months between the oldest and newest date in the ExpenseInfo table. This will also be true for non-reported expense data in the table.

- If the mode is Month you can assume you will be getting the first and last day of the  month as the @TimeFrameStart and @TimeFrameEnd parameters respectively. You do not need to check if the time frame will be between those months. 

# Sample Outputs*

## All Time 

exec ExpenseInformation @Mode = 'All'

| Mode          | DateRangeStart | DateRange End  | Employee | OverExpenseYN | OverNonReportYN | HigherThanAverageYN | SuspeciousYN |
| ------------- |:-------------: | :-----------:  |:--------:|:-------------:|:-------------:  | :-------------:     |:------------:|    
| All           | 01/01/2021     | 03/30/2021     | Adam     |      1        |      0          |       0             |    1         |
| All           | 01/01/2021     | 03/30/2021     | Michael  |      0        |      0          |       0             |    0         |
| All           | 01/01/2021     | 03/30/2021     | Katie    |      1        |      0          |       1             |    0         |

## Monthly  

exec ExpenseInformation @Mode = 'Month', @TimeFrameStart = '02/01/2021', @TimeFrameEnd = '02/28/2021'

| Mode          | DateRangeStart | DateRange End    | Employee | OverExpenseYN | OverNonReportYN | HigherThanAverageYN | SuspeciousYN |
| ------------- |:-------------: | :-----------:    |:--------:|:-------------:|:-------------:  | :-------------:     |:------------:|    
| Month           | 02/01/2021     | 02/28/2021     | Adam     |      0        |      1          |       0             |    0         |
| Month           | 02/01/2021     | 02/28/2021     | Michael  |      0        |      0          |       1             |    1         |
| Month           | 02/01/2021     | 02/28/2021     | Katie    |      0        |      1          |       1             |    1         |

## DateRange  

exec ExpenseInformation @Mode = 'DateRange', @TimeFrameStart = '03/07/2021', @TimeFrameEnd = '03/12/2021'

| Mode          | DateRangeStart | DateRange End   | Employee | OverExpenseYN | OverNonReportYN | HigherThanAverageYN | SuspeciousYN |
| ------------- |:-------------: | :-----------:   |:--------:|:-------------:|:-------------:  | :-------------:     |:------------:|    
| DateRange     | 03/07/2021     | 03/12/2021      | Adam     |      0        |      1          |       0             |    0         |
| DateRange     | 03/07/2021     | 02/12/2021      | Michael  |      1        |      0          |       1             |    0         |
| DateRange     | 03/07/2021     | 02/12/2021      | Katie    |      0        |      1          |       0             |    0         |

## Error**

exec ExpenseInformation @Mode = 'DateRange', @TimeFrameStart = '03/07/2021', @TimeFrameEnd = '03/13/2021'

| Mode          | Error Message                                           | 
| ------------- |:-------------:                                          |     
| DateRange     | Can only have a 5 day period for the DateRange Mode     | 

<sup> * </sup> None of the outputs above are reflective of any actual data. 
</br>
<sup> ** </sup> You can choose what you want the message to be, but be descriptive.