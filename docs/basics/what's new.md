# What's New

Below is a list of new endpoints, enhancements and changes made to the Gusto API recently. These are all accessible in demo but many of these new features are intended for [Gusto Embedded Payroll](https://gusto.com/embedded-payroll) customers. Please [apply for early access](https://gusto-embedded-payroll.typeform.com/to/iomAQIj3?typeform-source=gusto.com) if you’d like to learn more and use any of these Beta endpoints for production. *Note, this endpoint will require you to enter a different agreement with Gusto.* 

**Have a feature request? [Share it here](https://airtable.com/shrV9BbbCn8DFVJZ8).**

### Run Payroll:
- [calculate](https://gusto.stoplight.io/docs/api/b3A6MTI0MTUzODk-calculate-a-payroll-beta) / [submit](https://gusto.stoplight.io/docs/api/b3A6MTI0MTUzOTA-submit-payroll-beta) / [cancel payroll](https://gusto.stoplight.io/docs/api/b3A6MTI3MzMxMDM-cancel-a-payroll-beta)
- [create an off-cycle payroll](https://gusto.stoplight.io/docs/api/b3A6MTQ3MTExMjU-create-an-off-cycle-payroll-beta)
- [create](https://gusto.stoplight.io/docs/api/b3A6MTIyNTk2MzY-create-a-contractor-payment-beta) / [cancel contractor payment](https://gusto.stoplight.io/docs/api/b3A6MTQ3MTExMjI-cancel-a-contractor-payment-beta)
- create / [update a pay schedule](https://gusto.stoplight.io/docs/api/b3A6MTM3NTg2MDE-update-a-pay-schedule) (enable/disable autopilot only right now)
*Learn more about these endpoints and how to use them together here: [Gusto Embedded Payroll](https://gusto.stoplight.io/docs/api/ZG9jOjE0NzI2OTgy-new-gusto-embedded-payroll).*


### Company Onboarding:
- [create partner managed company](https://gusto.stoplight.io/docs/api/b3A6MTMzNjk4NzY-create-a-partner-managed-company-beta) (create a new Gusto company and authenticate)
- [create](https://gusto.stoplight.io/docs/api/b3A6MTI3NDgwOTU-create-an-admin-for-the-company) / [get](https://gusto.stoplight.io/docs/api/b3A6MTI3NDgwOTQ-get-all-the-admins-at-a-company) admins
- [create](https://gusto.stoplight.io/docs/api/b3A6MTQxMjg0MTE-create-a-company-bank-account) / [verify](https://gusto.stoplight.io/docs/api/b3A6MTQxMzc1MDE-verify-a-company-bank-account) / [get](https://gusto.stoplight.io/docs/api/b3A6MTQxMjg0MTA-get-all-company-bank-accounts) company bank accounts
- [update](https://gusto.stoplight.io/docs/api/b3A6MTU3ODY5MjY-update-federal-tax-details) / [get](https://gusto.stoplight.io/docs/api/b3A6MTU3ODY5MjU-get-federal-tax-details) federal tax details


### Employee Onboarding:
- [create](https://gusto.stoplight.io/docs/api/b3A6MTg1NTAzOTI-create-an-employee-bank-account) / [get](https://gusto.stoplight.io/docs/api/b3A6MTg1NTAzOTE-get-all-employee-bank-accounts) / [delete](https://gusto.stoplight.io/docs/api/b3A6MTg1NTAzOTM-delete-an-employee-bank-account) employee bank accounts
- [get](https://gusto.stoplight.io/docs/api/b3A6MTk0NjI2NTU-get-an-employee-s-payment-method) employee payment method
