---
tags: [signatory, sign company forms]
---

# Sign Company Forms

### Create a Signatory

**In order to sign company forms, you must first [create a signatory
](https://docs.gusto.com/docs/api/b3A6MjU1Mzg3MDE-create-a-signatory).**

The signatory should be an officer, owner, general partner or LLC member manager, plan administrator, fiduciary, or an authorized representative who is designated to sign agreements on the company's behalf. An officer is the president, vice president, treasurer, chief accounting officer, etc.

**There can only be a single primary signatory in a company** so you must [delete a signatory](https://docs.gusto.com/docs/api/b3A6MjU1Mzg3MDI-delete-a-signatory) before creating a new one. You can retrieve the current primary signatory from `GET /v1/companies/{company_id_or_uuid}` endpoint under the `primary_signatory` property.

### Sign Company Forms

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

After identifying which forms require signing (`"requires_signing": true`) you can take leverage `uuid` of the form and make a subsequent request to [Sign a company form](https://docs.gusto.com/docs/api/b3A6MjU1NDc5NzE-sign-a-company-form). This request should include:

- **signature_text** (string) - The name of the Company Signatory
- **agree** (boolean) - whether you agree to sign electronically
- **signed_by_ip_address** (string) - The IP address of the signatory who signed the form.

![](../../assets/images/Sign%20Forms.png)


