# Process Off-Cycle Payroll

[One-off (or 'off-cycle') payrolls](https://support.gusto.com/payroll/processing-payrolls/off-cycle-payrolls/999908231/Run-an-off-cycle-payroll.htm) are available if you need to pay your team outside of the regular payroll schedule. [Reasons for an off-cycle](https://support.gusto.com/payroll/processing-payrolls/off-cycle-payrolls/1019772541/Reasons-for-running-an-off-cycle-payroll.htm) can vary. Unlike regular payrolls, off-cycle payrolls are ad-hoc and do not have a set schedule.

To run an off-cycle payroll, the admin starts by entering the pay period start and end date. Key inputs include check date, any deductions and contributions, tax withholding rates, hours and additional earnings, and time off. Note that off-cycle payrolls default to using the same withholding rates, deductions, and contributions made in regular payrolls.

A POST request to the `create an off-cycle payroll` endpoint creates a new, unprocessed, off-cycle payroll and returns a unique `payroll_id`. 

To make any changes to the created off-cycle, call the `update a payroll by ID `endpoint (e.g adding additional hours to be paid or adding/increasing fixed_compensations)

Similar to processing a regular payroll, call the `calculate a payroll` endpoint using the unique `payroll_id` for the off-cycle. The calculated payroll details provide a preview of the actual values that will be used when the payroll is run.

To view the details of the calculated payroll, use the ``GET a single payroll by ID` endpoint with the *`show_calculation`* and *`includes`* parameters.

If everything looks accurate, a payroll can be processed with a request to the `submit payroll` API. Upon success, this request transitions the payroll to the processed state and initiates the transfer of funds. **This is a critical step to process payroll. A payroll is not finalized without calling this endpoint.**

The `cancel a payroll` endpoint can also revert a `processed` payroll back to the `unprocessed` state. 

*As a reminder, a payroll cannot be canceled after 3:30pm PST on the `payroll_deadline`. If a customer needs to cancel a payroll after this time frame they will need to contact support.*