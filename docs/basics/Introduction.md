# Introduction

## A developer's best friend

Welcome to Gusto's API documentation. This site serves as both a technical reference for the endpoints available and a general overview of payroll. We designed the objects and their properties to require as little domain-specific payroll knowledge as possible. We are dedicated to developer success by offering robust APIs, detailed documentation, [self-service tools](https://dev.gusto.com/), and [hands-on support](https://gusto.com/about/bd/developer-faqs-menu), so you can focus on building payroll products that solve customer problems. 

We currently do not support API access for Gusto customers looking to connect their own company systems directly to their Gusto account using the API. We hope to offer this in the future and appreciate your patience in the meantime.

*Please note that some endpoints require additional information and approval. The steps to obtain access to these endpoints are outlined in their respective sections.*

### Developing with Gusto: What to expect

![](../../assets/images/dev-portal-visual.png)

#### Onboard

To get started, sign up for an account on our [**Developer Portal**](https://dev.gusto.com) and onboard to our sandbox environment.

Once you’ve created an account and completed your Organization setup, you will have access to your unique API token from the Organizations tab. This is used for provisioning new Gusto accounts.

Next, create an application to obtain API credentials - or “*keys*” - to access our demo environment. To create an application, you will need to provide at least one(1) redirect URI. You can enter multiple redirects for a single application - please do this instead of creating multiple applications for each one. OAuth2 does not support wildcard URIs or URIs with fragments (e.g #).

**The application will generate a unique `client_id` and `secret` to be used for authentication.**

Then create a demo company so that you can explore our product and successfully connect your application to Gusto via OAuth2 to begin making calls to the API. Demo companies are automatically generated with company info, employees, and previous payrolls so you can begin testing right away. The [Authentication Example](https://docs.gusto.com/docs/api/docs/basics/Authentication.md) has everything you need to get started.

Once you have your API Token, `client_id`, `secret`, and a demo company, you’re ready to begin building your integration. Check out this [short video tutorial](https://www.loom.com/share/b374109a4f98499195e49f1e52330bc8) for an introduction on testing the Gusto API (in demo). 

You can invite team members to join your developer account, too. This is done from the Organizations tab under **Team**.

*This developer portal is new. If you already have demo access to Gusto’s API or a live Gusto integration, we’ve transferred your account details over to this dev portal and you can reset your password to then login from [here](https://dev.gusto.com/accounts/password/new)*.

#### Build

**The API keys accessible from Gusto's [Developer Portal](https://dev.gusto.com) are for our sandbox environment only ([https://api.gusto-demo.com](https://api.gusto-demo.com))**. Use these in tandem with your Gusto demo accounts to build your application or [Gusto Embedded Payroll](https://gusto.com/embedded-payroll) product. Our documentation outlines all existing endpoints.

If you are an app developer, please reach out to the Gusto Developer Relations team at developer@gusto.com once you create your dev account and request access to the [Partner Checklist](https://docs.google.com/spreadsheets/d/19ORmdNtOhnmAPPTYL8b-YnEcVmPM7oDp4OjR9bB83A0/edit?usp=sharing), make a copy for your company, and then use this to scope the build and ultimately submit it to Gusto for review.

#### Review

Gusto requires a quality review (“QA”) of all builds using our API before we issue production keys. This ensures everything will function properly and provide a best-in-class customer experience once live. 

The review process for an **[integrated App](https://docs.gusto.com/docs/api/docs/integration%20options/Build%20an%20Application.md)** requires the [partner checklist](https://docs.google.com/spreadsheets/d/19ORmdNtOhnmAPPTYL8b-YnEcVmPM7oDp4OjR9bB83A0/edit?usp=sharing) created in the previous stage to be completed and emailed to developer@gusto.com. 

The review process for a **[Gusto Embedded Payroll](https://gusto.com/embedded-payroll)** product is different and you will be working directly with our Partnerships team through this process.
 
Once your integration is approved, Gusto’s Developer Relations team will issue your production keys to your developer account to access at any time.

#### Launch
Move your build to production and make it available to customers.  You’ll want to update your website or marketplace and publish support content for visibility. Once you train your sales team, it’s time to get the word out.

#### Optimize
Performance starts with promotion — send press releases, post on social, and email your customers. You can track API usage and review feedback and errors to improve overtime.
