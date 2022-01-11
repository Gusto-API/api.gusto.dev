# Jobs and Compensations

### Jobs

Employees can be paid in a multitude of ways. Some employees are paid on an hourly basis, some are salaried, and some are paid in a combination of ways. What an employee gets paid is determined by their jobs and compensations in Gusto.

A job is effectively an employee's title or pay rate. Each `job_id` is unique to an employee and an employee can have multiple jobs. Two employees cannot share the same `job_id` even if they share the same title. The [job object](https://docs.gusto.com/docs/api/c2NoOjUyNjYzOTY-job) contains a compensation which determines how much an employee earns for the respective job.

An employee must be created before a job can be attributed to the employee. The request to [create a job](https://docs.gusto.com/docs/api/b3A6MzExMzczNDY-create-a-job) should include a `title`, `location_id`, and `hire_date`. The `title` is the name of the job. The location for the job determines where the company is liable for state taxes and should be created prior to creating the job using [create a company location](https://docs.gusto.com/docs/api/b3A6MTQ3MTExMTY-create-a-company-location). The `hire_date` reflects the employee's `hire_date` or the `effective_date` of the job if the employee has multiple jobs. 

````json
{
 "title": "Regional Manager",
 "location_id": 1363316536327002,
 "hire_date": "2021-01-01"
}
````


If an employee is no longer paid for a specific job, you can [delete a individual job](https://docs.gusto.com/docs/api/b3A6MzExMzczNDQ-delete-an-individual-job) that an employee holds. This will remove the job from the employee's list of jobs and that job will no longer appear in the payroll object when retrieving or updating a payroll. This is not to be confused with terminating an employee, which makes the employee ineligible for payroll. You cannot delete an employee's primary job. Instead, you'll need to update the primary job to the intended title/rate accordingly.

### Compensations

**Creating a job automatically creates a non-exempt, hourly compensation of $0/hr. Compensation for a job should be updated in subsequent API call.**

Compensations contain information on how much is paid out for a job. The job object can have multiple compensations to account for changes over time, such as merit increases, **but only one compensation is active at a time**. The `rate` field in the job object reflects the current compensation.

```json
{
  "id": 7757869441038000,
  "uuid": "d6d1035e-8a21-4e1d-89d5-fa894f9aff97",
  "version": "d0e719137f89ca3dd334dd4cc248ffbb",
  "employee_id": 7757869432666661,
  "employee_uuid": "948daac8-4355-4ece-9e2a-229898accb22",
  "current_compensation_id": 7757869444844981,
  "current_compensation_uuid": "ea8b0b90-1112-4f9d-bb93-bf029bc8537a",
  "payment_unit": "Year",
  "primary": true,
  "title": "Account Director",
  "compensations": [
    {
      "id": 7757869444844981,
      "uuid": "ea8b0b90-1112-4f9d-bb93-bf029bc8537a",
      "version": "994b75511d1debac5d7e2ddeae13679f",
      "payment_unit": "Year",
      "flsa_status": "Exempt",
      "job_id": 7757869441038000,
      "job_uuid": "d6d1035e-8a21-4e1d-89d5-fa894f9aff97",
      "effective_date": "2021-01-20",
      "rate": "78000.00"
    }
  ],
  "rate": "78000.00",
  "hire_date": "2020-01-20",
  "location_id": 7757727716657803,
  "location": {
    "id": 7757727716657803,
    "street_1": "412 Kiera Stravenue",
    "street_2": "Suite 391",
    "city": "San Francisco",
    "state": "CA",
    "zip": "94107",
    "country": "USA",
    "inactive": false
  }
}
```

When [updating a compensation](https://docs.gusto.com/docs/api/b3A6MzExMzczNjA-update-a-compensation), the following information is needed:
- **rate** (string) - The dollar amount paid per payment unit.
- **payment_unit** (string) - The unit accompanying the compensation rate. If the employee is an owner, rate should be `Paycheck`. Allowed values: `Hour` `Week` `Month` `Year` `Paycheck`
- **flsa_status** (string) - The FLSA status for this compensation. Salaried (`Exempt`) employees are paid a fixed salary every pay period. Salaried with overtime (`Salaried Nonexempt`) employees are paid a fixed salary every pay period, and receive overtime pay when applicable. Hourly (`Nonexempt`) employees are paid for the hours they work, and receive overtime pay when applicable. Owners (`Owner`) are employees that own at least twenty percent of the company. Allowed values: `Exempt` `Salaried` `Nonexempt` `Owner`

````json
{
 "version": "98jr3289h3298hr9329gf9egskt3kagri32qqgiqe3872",
 "rate": "25.00",
 "payment_unit": "Hour",
 "flsa_status": "Exempt"
}
````

### Using Jobs to Update Payroll

When it's time to process payroll and pass hours and earnings for employees, what you're doing in actuality is updating an unprocessed payroll.

When you retrieve an unprocessed payroll for a specific pay period, we return an array of `employee_compensations` including `employee_ids` and their eligible `fixed_compensations`, `hourly_compensations`, and `paid_time_off`. 

Within the `hourly_compensations` array, there are attributes for `Regular Hours`, `Overtime`, and` Double overtime` which include `hours` and `job_id` fields.

You would use the `job_id` within each Hours type to identify which job you are populating hours for. **This is essential in ensuring that a specific employee is being attributed the correct amount of hours for a given job, resulting in the correct payment and reporting.**

You can review our [Updating Payrolls Guide](https://docs.gusto.com/docs/api/ZG9jOjUyNzMzNjU-updating-payrolls) for additional information and a step by step tutorial.

```json
      "hourly_compensations": [
        {
          "name": "Regular Hours",
          "hours": "40.000",
          "job_id": 1,
          "compensation_multiplier": 1
        },
        {
          "name": "Overtime",
          "hours": "15.000",
          "job_id": 1,
          "compensation_multiplier": 1.5
        },
        {
          "name": "Double Overtime",
          "hours": "0.000",
          "job_id": 1,
          "compensation_multiplier": 2
        },
        {
          "name": "Regular Hours",
          "hours": "40.000",
          "job_id": 2,
          "compensation_multiplier": 1
        },
        {
          "name": "Overtime",
          "hours": "5.000",
          "job_id": 2,
          "compensation_multiplier": 1.5
        },
        {
          "name": "Double Overtime",
          "hours": "0.000",
          "job_id": 2,
          "compensation_multiplier": 2
        }
      ]
```

### Custom Earnings


In addition to the standard earning types — like `Bonus`, `Tips`, and `Commission` — you can [create a custom earning type](https://docs.gusto.com/docs/api/b3A6MzExMzczOTE-create-a-custom-earning-type) and name it whatever you like. Custom Earnings are created at the company level instead of being attributed to a specific employee, by creating an additional `fixed_compensation` to the [Payroll object](https://docs.gusto.com/docs/api/c2NoOjY2NjU5NTY-payroll). When updating payroll, the custom earning is able to be updated for every employee in the company.

Let's say you're a spa & salon and you track sales of hair products in addition to hourly wages and tips. You can create the custom earning, "Sales", and update this earning type so that it appears as a line item on the employee's paystub.

```json
{
      "employee_id": 1123581321345589,
      "excluded": false,
      "fixed_compensations": [
        {
          "name": "Sales",
          "amount": "200.00",
          "job_id": 1
        },
        {
          "name": "Bonus",
          "amount": "300.00",
          "job_id": 1
        },
        {
          "name": "Commissions",
          "amount": "0.00",
          "job_id": 1
        }
      ]
}
```
To [create a custom earning type](https://docs.gusto.com/docs/api/b3A6MzExMzczOTE-create-a-custom-earning-type), you just need the `company_id_or_uuid` and the `name` of the custom earning type.

```json
{
  "name": "Sales",
  "uuid": "f4dc8972-8830-4c07-b623-349a04b040d7"
}
```
