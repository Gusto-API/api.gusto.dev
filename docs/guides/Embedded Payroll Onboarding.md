# Embedded Payroll Onboarding

### Getting Started

This guide covers the steps to fully onboard a company for [Gusto Embedded Payroll](https://docs.gusto.com/docs/api/ZG9jOjE0NzI2OTgy-new-gusto-embedded-payroll), which includes passing employer (company) and employee data. A company and its employees must be fully onboarded before the end user can process payroll.


To onboard a company for payroll, key inputs include company details (e.g. legal name, EIN), location(s), bank accounts, and other information. To onboard an employee, key inputs include employee details (e.g. name, addresses), pay rates (salaried or hourly), tax withholding, and so on. In practice, company, compensations, addresses, and tax/withholding information are relatively static.

Although APIs exist for the majority of Company and Employee Onboarding steps, **the Employee and Company state tax setup steps do not exist as public APIs at this time and must be completed using Gusto’s co-branded [Onboarding Form](https://docs.gusto.com/docs/api/ZG9jOjI2ODQ4MTI5-embedded-payroll-onboarding#generate-a-link-to-access-gustos-onboarding-form)**. Ultimately, our goal is to eventually support all company and employee onboarding steps using APIs so partners have the option to use APIs and/or our pre-built flows to fully onboard a company and its employees.

*This guide will be updated on a recurring basis as we release more API functionality.*

### Creating a Company and Authenticating

Embedded payroll begins with the creation of a company that you can manage. You’ll use the [Partner Managed Companies](https://docs.gusto.com/docs/api/b3A6MTMzNjk4NzY-create-a-partner-managed-company-beta) endpoint to:

- Create a new company in Gusto
- Create a new user in Gusto
- Make the new user the primary payroll administrator of the new company

**Authentication**

Due to the nature of this endpoint, Gusto will provide partners with an API token and will permit partners to use API Token Authentication instead of OAuth to provision Gusto accounts. The API token is included in the authorization HTTP header with the Token scheme, e.g.:

```json
Content-Type: application/json
Authorization: Token bbb286ff1a4fe6b84742b0d49b8d0d65bd0208d27d3d50333591df71
```

You will need the user’s `first_name`, `last_name`, `email`, and `company_name` in order to make this request.

Upon successful creation of the company, this creates a link between you (the partner) and the company. In the API response you will receive the access token, refresh token, and uuid of the company created. The access token and refresh token can be used immediately to authenticate. 

### Passing Company Information via API

The bearer token should be specified via the Authorization HTTP header for these requests.


After creating the company you can now pass any company information that you already collect using our API. If you do not already collect this information, you can build a UI in your app so the user can enter this information to pass along to Gusto. This information will pre-populate fields in the co-branded Onboarding Form that Gusto provides. **Company state tax setup does not exist as a public API at this time and must be completed using Gusto’s co-branded Onboarding Form.**

Using the `company_uuid` from the response of the [partner_managed_companies](https://docs.gusto.com/docs/api/b3A6MTMzNjk4NzY-create-a-partner-managed-company-beta) endpoint, you can make the following API calls to create and update company information:

[Create a company location
](https://docs.gusto.com/docs/api/b3A6MTQ3MTExMTY-create-a-company-location)

We’ll need the company’s mailing and filing address and all addresses where the company has employees physically working in the United States. This includes remote employees and employees who work from home. You can specify if the address you are creating is the `mailing_address` or `filing_address` in your request ie. `“mailing_address”: “true”`

[Update Federal Tax Details
](https://docs.gusto.com/docs/api/b3A6MTU3ODY5MjY-update-federal-tax-details)

This endpoint is used to update attributes relevant for a company’s federal taxes. We need the company's federal tax details so we can file and pay taxes correctly. You can find this info on the company's [IRS Form CP-575](https://gusto.com/blog/taxes/form-cp-575). You will need to provide the company’s `legal_name`, `ein`, `tax_payer_type`, `filing_form` (941 or 944), and whether this company should be `taxable_as_scorp`.

Allowed `tax_payer_type` values:
- C-Corporation
- S-Corporation
- Sole proprietor
- LLC
- LLP
- Limited partnership
- Co-ownership
- Association
- Trusteeship
- General partnership
- Joint venture
- Non-Profit

[Update a company industry selection
](https://docs.gusto.com/docs/api/b3A6MjU3MDkxNTI-update-a-company-industry-selection)

This endpoint is used to update the company’s industry selection using naics_code and sic_codes. Gusto needs this information to stay compliant with finance regulations. [North American Industry Classification System](https://www.naics.com/) (NAICS) is used to classify businesses with a six digit number based on the primary type of work the business performs. SIC codes are the list of [Standard Industrial Classification](https://siccode.com/), which are four digit numbers that categorize the industries that companies belong to based on their business activities. You can look up a company’s NAICS and SIC codes [here](https://www.naics.com/naics-code-description/).

[Create a company bank account
](https://docs.gusto.com/docs/api/b3A6MTQxMjg0MTE-create-a-company-bank-account)

This endpoint creates a new company bank account. If a default bank account exists, the new bank account will replace it as the company's default funding method.  Required fields to make this request include `routing_number`, `account_number`, and `account_type` (`Checking`, `Savings`).

Upon being created, two verification deposits are automatically sent to the bank account in **1-2 business days**, and the bank account's `verification_status` is `awaiting_deposits`.

When the deposits are successfully transferred, the `verification_status` changes to `ready_for_verification`, at which point the verify endpoint can be used to verify the bank account. After successful verification, the bank account's `verification_status` is `verified`.

[Create a new single pay schedule
](https://docs.gusto.com/docs/api/b3A6MjA5MTEyMTI-create-a-new-single-pay-schedule)

This creates one pay schedule during company onboarding and cannot be used if the company has processed a payroll. Creating multiple pay schedules at this time is not supported. To change a pay schedule, the end user will need to login to Gusto to edit their pay schedule. Be sure to check [state laws](https://www.dol.gov/agencies/whd/state/payday) to know what schedule is right for your customers.
When setting a pay schedule, the following information is needed:

- **frequency** (string) - The frequency that employees on this pay schedule are paid with Gusto. Allowed values: `Every week` `Every other week` `Twice per month` `Monthly`
- **anchor_pay_date** (string) - the first date that employees on this pay schedule are paid with Gusto
- **anchor_end_of_pay_period** (string) - The last date of the first pay period. This can be the same date as the anchor pay date.
- **day_1** and **day_2**: (integer between 1 and 31 or null) Indicates the first day of the month that employees are paid. This field is only relevant for pay schedules with the `Twice per month` and `Monthly` frequencies. It will be null for pay schedules with other frequencies.

[Create a signatory
](https://docs.gusto.com/docs/api/b3A6MjU1Mzg3MDE-create-a-signatory)

This endpoint creates a company signatory who can legally sign forms. The company signatory is someone, usually an executive, designated to sign agreements on the company's behalf. Please note that there can only be a single primary signatory in a company. 

[Verify a company bank account
](https://docs.gusto.com/docs/api/b3A6MTQxMzc1MDE-verify-a-company-bank-account)

After creating the company bank account, Gusto will automatically issue two verification deposits to the bank account, and the bank account's `verification_status` will be `awaiting_deposits`. **This can take 1-2 business days**. When the deposits are successfully transferred, the `verification_status` changes to `ready_for_verification`, at which point the verify endpoint can be used to verify the bank account. After successful verification, the bank account's `verification_status` is `verified`.

The order of the two deposits specified in request parameters does not matter. There's a maximum of 5 verification attempts, after which we will automatically initiate a new set of micro-deposits and require the bank account to be verified with the new micro-deposits.

**Note: Due to the nature in which our demo environment behaves, there is a separate endpoint for verifying the company bank account in demo**. [This endpoint](https://docs.gusto.com/docs/api/b3A6MTQxMzc1MDE-verify-a-company-bank-account) simulates the micro-deposits transfer and returns them in the response. You can call this endpoint as many times as you wish to retrieve the values of the two micro deposits.


### Passing Employee Information via API

The bearer token should be specified via the Authorization HTTP header for these requests.


After creating the company you can now pass any employee information that you already collect using our API. If you do not already collect this information, you can build a UI in your app so the user can enter this information to pass along to Gusto. This information will pre-populate fields in the co-branded Onboarding Form that Gusto provides. **Employee state tax setup does not exist as a public API at this time and must be completed using Gusto’s co-branded Onboarding Form.** 

Using the `company_uuid` from the response of the `partner_managed_companies` endpoint, you can make the following API calls to create and update employee information:

[Create an employee
](https://docs.gusto.com/docs/api/b3A6MTQ3MTExMTQ-create-an-employee)

You can use this endpoint to create an employee record in Gusto using basic employee information you already collect. This includes `first_name`, `middle_initial`, `last_name`, `date_of_birth`, `email` and `ssn`. After the employee is created, subsequent calls can be made to update additional information like their `home_address`, `employee_bank_account`, `employee_payment_method`, `jobs`, and `compensations`.

[Update an employee’s home address
](https://docs.gusto.com/docs/api/b3A6MTE3OTYxOTM-update-an-employee-s-home-address)

The home address of an employee is used to determine certain tax information about them. Gusto only supports W2 employees based in the US at this time.

[Create an employee bank account
](https://docs.gusto.com/docs/api/b3A6MTg1NTAzOTI-create-an-employee-bank-account)

This endpoint creates an employee bank account. Required fields to make this request include name, `routing_number`, `account_number`, and `account_type` (`Checking`, `Savings`). An employee can have multiple bank accounts and you can determine how an employee’s pay is split by updating the employee’s payment method. Creating an employee bank account will automatically update an employee’s payment method from `Check` to `Direct Deposit`.

[Update an employee’s payment method
](https://docs.gusto.com/docs/api/b3A6MTk0NjI2NTY-update-an-employee-s-payment-method)

This endpoint can be called to update an employee’s payment method or update how they want to split their pay if they’ve set up multiple employee bank accounts. Payments can be split by an `Amount` (dollar amount) or by a `Percentage`. You can also set the payment priority, which is the order of priority for each payment split. `"priority": 1` would be the first bank account to receive payment, for example.

[Create a job
](https://docs.gusto.com/docs/api/b3A6MTAwMDM4Njg-create-a-job)

A job is the representation of an employee’s title or pay rate. An employee can have multiple jobs in Gusto and each job_id is unique to an employee. Two employees cannot share the same job_id. The request should include a title, location_id, and hire_date. The location needs to be created in advance using [Create a company location](https://docs.gusto.com/docs/api/b3A6MTQ3MTExMTY-create-a-company-location). The hire_date can be the employee’s actual hire_date or the effective_date of the job if the employee has multiple jobs. 

Creating a job automatically creates a non-exempt, hourly compensation of $0/hr. Compensation for a job should be updated in subsequent API call.

[Update a compensation
](https://docs.gusto.com/docs/api/b3A6NTU0OTY5MQ-update-a-compensation)

Compensations contain information on how much is paid out for a job. Jobs may have many compensations, but only one that is active. The current compensation is the one with the most recent effective_date.

When updating a compensation, the following information is needed:
- **rate** (string) - The dollar amount paid per payment unit.
- **payment_unit** (string) - The unit accompanying the compensation rate. If the employee is an owner, rate should be 'Paycheck'. Allowed values: `Hour` `Week` `Month` `Year` `Paycheck`
- **flsa_status** (string) - The FLSA status for this compensation. Salaried ('Exempt') employees are paid a fixed salary every pay period. Salaried with overtime ('Salaried Nonexempt') employees are paid a fixed salary every pay period, and receive overtime pay when applicable. Hourly ('Nonexempt') employees are paid for the hours they work, and receive overtime pay when applicable. Owners ('Owner') are employees that own at least twenty percent of the company. Allowed values: `Exempt` `Salaried` `Nonexempt` `Nonexempt` `Owner`

### Deleting an Employee

If at any point you need to delete an employee who is in the middle of onboarding you can call the [Delete an onboarding employee](https://docs.gusto.com/docs/api/b3A6MjU3MTM4NDQ-delete-an-onboarding-employee) endpoint. This is only intended to delete an employee that has not been fully onboarded, as deleting an onboarded employee “onboarded”: “true” is not permitted.

### Generate a link to access Gusto’s Onboarding Form

After you’ve passed all of the company and employee information you collect using our API, you will generate a link to access Gusto’s Onboarding Form using the [Create a flow](https://docs.gusto.com/docs/api/b3A6MjUxNjcyODY-create-a-flow) endpoint so that the user can complete any remaining steps. If no company or employee information was passed in advance using our API, all required fields can be input by the user via the form.

```json
{
  "flow_type": "company_onboarding",
  "entity_type": "Company",
  "entity_uuid": "company_uuid"
}
```

![](../../assets/images/Onboarding%20Form%20Sample.png)

Employee state tax details and Company state tax details, do not exist as public APIs and therefore must be completed using Gusto’s Onboarding Form. State tax information will be needed for every state that an employee is physically working from. Each state has its own requirements when it comes to IDs and tax rates. You can reference our [state tax table](https://support.gusto.com/hub/Employers-and-admins/Taxes-forms-and-compliance) to determine what information is needed for each state.

### Company Forms

We require the Company Signatory to sign forms that authorize Gusto to file forms and make payroll tax deposits on the company's behalf. **Forms can be signed through Gusto's [Onboarding Form](https://docs.gusto.com/docs/api/ZG9jOjI2ODQ4MTI5-embedded-payroll-onboarding#generate-a-link-to-access-gustos-onboarding-form) or via API calls.**

These are the prerequisites in order to `GET` and sign Company Forms:
- `"add_employees"`
- `"federal_tax_setup"`
- `"state_setup"` (via Gusto's Onboarding Form)
- `"add_bank_info"`
- `"payroll_schedule"`

After these steps are completed you can [Get all company forms](https://docs.gusto.com/docs/api/b3A6MjU1NDc5Njg-get-all-company-forms). In the API response you'll receive:

- **uuid** (string) - The UUID of the form
- **name** (string) - The type identifier of the form
- **title** (string) - The title of the form
- **description** (string) - The description of the form
- **requires_signing** (boolean) - A boolean flag that indicates whether the form needs signing or not. Note that this value will change after the form is signed.

After identifying which forms require signing, you can take leverage `uuid` of the form and make a subsequent request to [Sign a company form](https://docs.gusto.com/docs/api/b3A6MjU1NDc5NzE-sign-a-company-form). This request should include:

- **signature_text** (string) - The name of the Company Signatory
- **agree** (boolean) - whether you agree to sign electronically
- **signed_by_ip_address** (string) - The IP address of the signatory who signed the form.

### Completing Onboarding and Company Approval

You can check the company’s onboarding status by pinging our [Get the company’s onboarding status](https://docs.gusto.com/docs/api/b3A6MjU3Mjg5NTQ-get-the-company-s-onboarding-status) endpoint. The data returned in this endpoint helps inform the required onboarding steps and respective completion status.

````json
{
  "uuid": "c44d66dc-c41b-4a60-9e25-5e93ff8583f2",
  "onboarding_completed": false,
  "onboarding_steps": [
    {
      "title": "Add Your Company's Addresses",
      "id": "add_addresses",
      "required": true,
      "completed": true,
      "requirements": []
    },
    {
      "title": "Add Your Employees",
      "id": "add_employees",
      "required": true,
      "completed": true,
      "requirements": [
        "add_addresses"
      ]
    },
    {
      "title": "Enter Your Federal Tax Information",
      "id": "federal_tax_setup",
      "required": true,
      "completed": true,
      "requirements": [
        "add_addresses",
        "add_employees"
      ]
    },
    {
      "title": "Add Your Bank Account",
      "id": "add_bank_info",
      "required": true,
      "completed": true,
      "requirements": []
    },
    {
      "title": "Select a Pay Schedule",
      "id": "payroll_schedule",
      "required": true,
      "completed": false,
      "requirements": []
    },
    {
      "title": "Sign Documents",
      "id": "sign_all_forms",
      "required": true,
      "completed": false,
      "requirements": [
        "add_employees",
        "federal_tax_setup",
        "state_setup",
        "add_bank_info",
        "payroll_schedule"
      ]
    },
    {
      "title": "Verify Your Bank Account",
      "id": "verify_bank_info",
      "required": true,
      "completed": false,
      "requirements": [
        "add_bank_info"
      ]
    }
  ]
}
````

After completing onboarding, the company will be reviewed by Gusto’s internal approval process which can take 1-2 business days. A company must be approved before it can process payroll. You can check the company’s approval status using our [Get a company endpoint](https://docs.gusto.com/docs/api/b3A6MTQ3MTExMTI-get-a-company) where `"company_status": "Approved"`.

### Video Tutorial

See the APIs and Onboarding Form in action via this [Loom tutorial](https://www.loom.com/share/85a3dfb5d1f64e1389a56cbb0ef4d37b).
