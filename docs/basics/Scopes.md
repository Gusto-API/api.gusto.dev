# Scopes

API Scopes control access to various API endpoints for your app. Gusto uses scopes that refer to the resource they grant access to, followed by the action on that resource they allow (e.g. `employees:read`).

We currently support two classes of action:
-  read: Reading the information about a resource.
- write: Modifying the resource in any way e.g. creating, editing, or deleting.

During review, all applications will be assigned API scopes based on your integration use-case. These scopes must be tested in our demo environment and will be enforced in our production environment upon approval. Any API call made outside of your assigned scope in production will be rejected. You'll be able to request additional scopes or changes to your scopes by contacting developer@gusto.com after releasing to production.

You can check your app’s granted API scopes via your Developer Portal account. Sign in [here](https://dev.gusto.com/accounts/sign_in)

It’s recommended to limit the scopes you need to the bare minimum so that users can feel confident with your app and the amount of data it can access.
