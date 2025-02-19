With directory group-to-team provisioning from your IdP, user updates will automatically sync with your Docker organizations and teams.

To correctly assign your users to Docker teams, you must create groups in your IDP following the naming pattern `organization:team`. For example, if you want to manage provisioning for the team "developers” in Docker, and your organization name is “moby,” you must create a group in your IdP with the name “moby:developers”.

Once you enable group mappings in your connection, users assigned to that group in your IdP will automatically be added to the team “developers” in Docker.

>**Tip**
>
>Use the same names for the Docker teams as your group names in the IdP to prevent further configuration. When you sync groups, a group is created if it doesn’t already exist.
{: .tip}

## How group mapping works

IdPs share with Docker the main attributes of every authorized user through SSO, such as email address, name, surname, and groups. These attributes are used by Just-In-Time (JIT) Provisioning to create or update the user’s Docker profile and their associations with organizations and teams on Docker Hub.

Docker uses the email address of the user to identify them on the platform. Every Docker account must have a unique email address at all times.

After every successful SSO sign-in authentication, the JIT provisioner performs the following actions:

1. Checks if there's an existing Docker account with the email address of the user that just authenticated.

    a) If no account is found with the same email address, it creates a new Docker account using basic user attributes (email, name, and surname). The JIT provisioner generates a new username for this new account by using the email, name, and random numbers to make sure that all account usernames are unique in the platform.

    b) If an account exists for this email address, it uses this account and updates the full name of the user’s profile if needed.
2. Checks if the IdP shared group mappings while authenticating the user.

    a) If the IdP provided group mappings for the user, the user gets added to the organizations and teams indicated by the group mappings.

    b) If the IdP didn't provide group mappings, it checks if the user is already a member of the organization, or if the SSO connection is for multiple organizations (only at company level) and if the user is a member of any of those organizations. If the user is not a member, it adds the user to the default team and organization configured in the SSO connection.

![JIT provisioning](/docker-hub/images/jit.PNG)

## Use group mapping

To take advantage of group mapping, follow the instructions provided by your IdP:

- [Okta](https://help.okta.com/en-us/Content/Topics/users-groups-profiles/usgp-enable-group-push.htm){: target="_blank" rel="noopener" class="_" }
- [Azure AD](https://learn.microsoft.com/en-us/azure/active-directory/app-provisioning/customize-application-attributes){: target="_blank" rel="noopener" class="_" }
- [OneLogin](https://developers.onelogin.com/scim/create-app){: target="_blank" rel="noopener" class="_" }

Once complete, a user who signs in to Docker through SSO is automatically added to the organizations and teams mapped in the IdP.