# What's New

Below is a list of new endpoints, enhancements and changes made to the Gusto API recently. These are all accessible in demo but many of these new features are intended for [Gusto Embedded Payroll](https://gusto.com/embedded-payroll) customers. Please [apply for early access](https://gusto-embedded-payroll.typeform.com/to/iomAQIj3?typeform-source=gusto.com) if you’d like to learn more and use any of these Beta endpoints for production. *Note, this endpoint will require you to enter a different agreement with Gusto.* 

**Have a feature request? [Share it here](https://airtable.com/shrV9BbbCn8DFVJZ8).**

### Run Payroll:
- [May 2021] [calculate](https://gusto.stoplight.io/docs/api/b3A6MTI0MTUzODk-calculate-a-payroll-beta) / [submit](https://gusto.stoplight.io/docs/api/b3A6MTI0MTUzOTA-submit-payroll-beta) / [cancel payroll](https://gusto.stoplight.io/docs/api/b3A6MTI3MzMxMDM-cancel-a-payroll-beta)
- [May 2021] [create an off-cycle payroll](https://gusto.stoplight.io/docs/api/b3A6MTQ3MTExMjU-create-an-off-cycle-payroll-beta)
- [May 2021] [create](https://gusto.stoplight.io/docs/api/b3A6MTIyNTk2MzY-create-a-contractor-payment-beta) / [cancel contractor payment](https://gusto.stoplight.io/docs/api/b3A6MTQ3MTExMjI-cancel-a-contractor-payment-beta)
- [Sept 2021] [create](https://docs.gusto.com/docs/api/b3A6MjA5MTEyMTI-create-a-new-single-pay-schedule) / [update a pay schedule](https://gusto.stoplight.io/docs/api/b3A6MTM3NTg2MDE-update-a-pay-schedule) (enable/disable autopilot only right now)
- [Nov 2021] [skip payroll](https://docs.gusto.com/docs/api/b3A6MTQ3MTExMjg-update-a-payroll) (`"excluded": "true/false"` param under `"employee_compensations"`)
- [Nov 2021] [get employee pay stub (pdf)](https://docs.gusto.com/docs/api/b3A6MjgwMzMyOTE-get-an-employee-pay-stub-pdf)

*Learn more about these endpoints and how to use them together here: [Gusto Embedded Payroll](https://gusto.stoplight.io/docs/api/ZG9jOjE0NzI2OTgy-new-gusto-embedded-payroll).*


### Company Onboarding:
- [May 2021] [create partner managed company](https://gusto.stoplight.io/docs/api/b3A6MTMzNjk4NzY-create-a-partner-managed-company-beta) (create a new Gusto company and authenticate)
- [May 2021] [create](https://gusto.stoplight.io/docs/api/b3A6MTI3NDgwOTU-create-an-admin-for-the-company) / [get](https://gusto.stoplight.io/docs/api/b3A6MTI3NDgwOTQ-get-all-the-admins-at-a-company) admins
- [June 2021] [create](https://gusto.stoplight.io/docs/api/b3A6MTQxMjg0MTE-create-a-company-bank-account) / [verify](https://gusto.stoplight.io/docs/api/b3A6MTQxMzc1MDE-verify-a-company-bank-account) / [get](https://gusto.stoplight.io/docs/api/b3A6MTQxMjg0MTA-get-all-company-bank-accounts) company bank accounts
- [Aug 2021] [update](https://gusto.stoplight.io/docs/api/b3A6MTU3ODY5MjY-update-federal-tax-details)  /  [get](https://gusto.stoplight.io/docs/api/b3A6MTU3ODY5MjU-get-federal-tax-details) federal tax details
- [Oct 2021] [create](https://docs.gusto.com/docs/api/b3A6MjU1Mzg3MDE-create-a-signatory) / [delete](https://docs.gusto.com/docs/api/b3A6MjU1Mzg3MDI-delete-a-signatory) a signatory
- [Oct 2021] [get all company forms](https://docs.gusto.com/docs/api/b3A6MjU1NDc5Njg-get-all-company-forms)
- [Oct 2021] [get](https://docs.gusto.com/docs/api/b3A6MjU1NDc5Njk-get-a-form) / [get a pdf](https://docs.gusto.com/docs/api/b3A6MjU1NDc5NzA-get-a-form-pdf) / [sign](https://docs.gusto.com/docs/api/b3A6MjU1NDc5NzE-sign-a-company-form) company form
- [Oct 2021] [update company industry selection](https://docs.gusto.com/docs/api/b3A6MjU3MDkxNTI-update-a-company-industry-selection)
- [Oct 2021] [get company onboarding status](https://docs.gusto.com/docs/api/b3A6MjU3Mjg5NTQ-get-the-company-s-onboarding-status)
- [Oct 2021] [finish company onboarding](https://docs.gusto.com/docs/api/b3A6MjU3Mjg5NTU-finish-company-onboarding)
- [Oct 2021] [create onboarding flow](https://docs.gusto.com/docs/api/b3A6MjUxNjcyODY-create-a-flow) (for Gusto Embedded Payroll)

### Employee Onboarding:
- [Sept 2021] [create](https://gusto.stoplight.io/docs/api/b3A6MTg1NTAzOTI-create-an-employee-bank-account) / [get](https://gusto.stoplight.io/docs/api/b3A6MTg1NTAzOTE-get-all-employee-bank-accounts) / [delete](https://gusto.stoplight.io/docs/api/b3A6MTg1NTAzOTM-delete-an-employee-bank-account) employee bank accounts
- [Sept 2021] [update](https://docs.gusto.com/docs/api/b3A6MTk0NjI2NTY-update-an-employee-s-payment-method) / [get](https://gusto.stoplight.io/docs/api/b3A6MTk0NjI2NTU-get-an-employee-s-payment-method) employee payment method
- [Nov 2021] [update employee's federal taxes](https://docs.gusto.com/docs/api/b3A6MzEyOTc1OTQ-update-an-employee-s-federal-taxes)
- [Nov 2021] [delete onboarding employee](https://docs.gusto.com/docs/api/b3A6MjU3MTM4NDQ-delete-an-onboarding-employee)
- [Nov 2021] [finish onboarding employee](https://docs.gusto.com/docs/api/b3A6MjU3MTM4NDU-finish-onboarding-an-employee)

### Employees
- [Oct 2021] Added `current_employment_status` to [employee object](https://docs.gusto.com/docs/api/c2NoOjUyMTU4NTA-employee)


### Pagination:
- [Oct 2021] Landed [pagination](https://docs.gusto.com/docs/api/ZG9jOjIzNzA2ODY5-pagination) to all collection endpoints on the public api. This as an opt-in feature for version 1.
