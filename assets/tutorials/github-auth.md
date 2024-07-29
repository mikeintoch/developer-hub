# Configure GitHub Access to Red Hat Developer Hub

## Set up simple GitHub authentication

Only for learning purposes, let’s set up simple authentication to GitHub.

You need a GitHub account with some options set up on GitHub itself. 

The simplest way (which is not recommended for production because you would not normally put tokens and secrets directly into the config) is quick and easy, but doesn’t show you how to set up a GitHub organization. To use this method, do the following:

1. Go to [https://github.com](https://github.com) and log in with your GitHub credentials.
2. Visit [https://github.com/settings/tokens/new](https://github.com/settings/tokens/new) to generate a new "classic" token..
3. Now give the Token a name you will remember in the Note box.
4. Tick the **Repo** box to allow this token access to the repositories. Doing this allows Developer Hub to write a repo, which is part of the lesson.
5. Select **Generate Token** and **Copy the token details and save them.** You will not see them again. This is your **ACCESS_TOKEN**.
6. Now visit [https://github.com/settings/apps/new](https://github.com/settings/apps/new) to register a new OAuth App.

> Info alert:Make sure you have created an OAuth App, not a Github App.

7. Enter your developer hub application route URL in the Homepage URL input. This value is the default URL you are taken to when you enter Red Hat Developer Hub, which we suggested you note in the installation phase. You can confirm this URL by going to the roundel and clicking the **Route** icon.
8. In the OAuth App creation page, enter a memorable name.
9. Ensure that the Callback URL in your GitHub application is configured as follows:
https://redhat-developer-hub-<NAMESPACE_NAME\>.<OPENSHIFT_ROUTE_HOST\>/api/auth/github/handler/frame
10. Under Webhook, uncheck the Active box.
11. Click Create GitHub App to create the app. This will take you to the About screen.
12. Under Client Secrets, click Generate a new client secret.
13. Copy and save both CLIENT_ID and Client Secrets.

For the next part of the setup, you can leave the GitHub website and return to the Developer Sandbox. Haveyour **ACCESS_TOKEN**, **CLIENT_ID**, and Client Secrets information ready.

At this point, let's change the configuration of our installed Developer Hub to use the information we just generated for the GitHub authentication:

1. Download Git Repo with our configuration files. Execute in Developer Sandbox Terminal following command:

```shell
git clone https://github.com/mikeintoch/developer-hub.git
``` 
- Open file cm-app-config-rhdh.yaml in the assets/config folder and replace all of the values within curly braces ({ }) with the data you generated, and make sure you delete the curly braces when you are finished.

```yaml
kind: ConfigMap
apiVersion: v1
metadata:
  name: app-config-rhdh
data:
  app-config-rhdh.yaml: |
    app:
      title: My Red Hat Developer Hub
      baseUrl: https://redhat-developer-hub-{your_sandbox_project}.apps.{your_sandbox_url}.openshiftapps.com/
    integrations:
     github:
       - host: github.com
         token: {YOUR ACCESS_TOKEN}
    auth:
     allowGuestAccess: true
     environment: development
     providers:
       github:
         development:
           clientId: {YOUR CLIENT_ID}
           clientSecret: {YOUR Client Secrets}
    enabled:
      github: true
```

2. Save the file.
3. Create new ConfigMap using Developer Sandbox Terminal with following command:

```shell
oc create -f ./developer-hub/assets/config/cm-disable-auth.yaml
```
> If you disable default authentication you only run following commands to update your configuration:
> ```shell
> oc replace -f ./developer-hub/assets/config/cm-app-config-rhdh.yaml
> oc rollout restart deployment/redhat-developer-hub
> ```

4. Upgrade helm chart configuration adding new config map

```shell
helm upgrade --reuse-values redhat-developer-hub openshift-helm-charts/redhat-developer-hub \ 
--set "upstream.backstage.extraAppConfig[0].configMapRef=app-config-rhdh" \ 
--set "upstream.backstage.extraAppConfig[0].filename=app-config-rhdh.yaml"
```

5. Once the application has redeployed. The application will restart and the new contents of the ConfigMap will be injected.
6. Go back and refresh the web page with the Developer Hub portal.
7. Go to the **Settings** gear (bottom left).

<img src="/assets/images/5.png" width="70%" height="70%" style="display: block; margin: 0 auto">

8. Under **Profile** (which should say Guest), click the three-button icon, then select Sign Out. This will take you to a login page that has two options.

<img src="/assets/images/19.png" width="70%" height="70%" style="display: block; margin: 0 auto">

Now choose GitHub to log in and follow the prompts. If your configuration is correct, you will be logged back onto the homepage. Select Settings in the bottom left. The screen should now look similar to the one following figure. 

You are now ready to show the capabilities of Red Hat Developer Hub.



