# Interview Question
In this scenario a client wants to see a preview of what their employees
payroll totals will be as well as the total for everyone.

## Expectations
1. Please strengthen the dataSetup to make sure any employees being inserted into
the Payroll table exist in the Employees table first.
2. This report should summarize each employees totals broken down by paycode.
3. It should also give the total amount for the entire payroll.

# Sample Output*

exec PayrollSummary

| Employee     | Paycode     | Total    |
|--------------|:-----------:|:--------:|
| John Doe     | Regular     | 607.85   |
| John Doe     | Overtime    | 52.50    |
| Matt Smith   | Regular     | 239.29   |
| David Evans  | Salary      | 2100.00  |
| Jane Schmidt | Regular     | 894.68   |
| Jane Schmidt | Overtime    | 82.94    |
| Chase Jones  | Salary      | 1566.67  |
| Total        |             | 5543.93  |