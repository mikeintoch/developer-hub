# Integrate Microcks with Red Hat Developer Hub

# Prerequirements.

Before you start this tutorial, First you need to configure authentication

You can follow two different paths.

- [Disable Default authentication.](/assets/tutorials/install-dev-hub.md#disable-authentication-optional)

or

- [Configure Github Authentication](/assets/tutorials/github-auth.md)

### Install Microcks

On Terminal run following commands.

```
oc create -f https://gist.githubusercontent.com/mikeintoch/895106c7c1005b34a157c7ae33835b83/raw/
```

As an output you install Microcks with non-persistent configuration.

<img src="/assets/images/2.png" width="50%" height="50%" style="display: block; margin: 0 auto">
---

## Create Mock API's with Microcks

- Open Microcks console
<img src="/assets/images/7.png" width="70%" height="70%" style="display: block; margin: 0 auto">

- Click on **Microcks Hub**
<img src="/assets/images/8.png" width="30%" height="30%" style="display: block; margin: 0 auto">

- Click on **MicrocksIO Samples API**
<img src="/assets/images/9.png" width="70%" height="70%" style="display: block; margin: 0 auto">

- Click on **Pastry API**
<img src="/assets/images/10.png" width="70%" height="70%" style="display: block; margin: 0 auto">

- Click on **Install**
<img src="/assets/images/11.png" width="70%" height="70%" style="display: block; margin: 0 auto">

- Click on **Direct Import**
<img src="/assets/images/12.png" width="70%" height="70%" style="display: block; margin: 0 auto">

- API is successfully installed
<img src="/assets/images/13.png" width="70%" height="70%" style="display: block; margin: 0 auto">

- Click on **APIs|Services** and you can see API Pastry listed.
<img src="/assets/images/14.png" width="70%" height="70%" style="display: block; margin: 0 auto">

---
## Install Microcks Plugin on Developer Hub.

- Execute in Developer Sandbox Terminal following commands

```shell
git clone https://github.com/mikeintoch/developer-hub.git

```
- Perform values on file microcks-plugin-install.yaml change MICROCKS_URL

```shell
...
            providers:
              microcksApiEntity:
                dev:
                  addOpenAPIServerUrl: true
                  baseUrl: https://MICROCKS_URL
                  ownerLabel: team
                  schedule:
                    frequency:
...
```
- Run following command on Developer Sandbox Terminal.

```shell
cd developer-hub

sed -i -e "s/MICROCKS_URL/$(oc get route microcks-uber --output jsonpath={.spec.host})/g" assets/config/microcks-plugin-install.yaml
```

- Upgrade helm chart with new configuration.

```shell
cd developer-hub

helm upgrade --reuse-values redhat-developer-hub openshift-helm-charts/redhat-developer-hub -f assets/config/microcks-plugin-install.yaml
```

- Once the application has redeployed. The application will restart and Developer Hub is ready to wirk with Microcks.
---

## Validate integration with Microcks and Red Hat Developer Hub

Go back and refresh the web page with the Developer Hub portal.

Now Developer Hub is able to show APIs you create on Microcks through plugin previously installed.

- Open your Red Hat Developer Hub console in the left menu click on APIs
<img src="/assets/images/15.png" width="30%" height="30%" style="display: block; margin: 0 auto">

- RHDH shows APIs listed then click on the name of the API.
<img src="/assets/images/16.png" width="70%" height="70%" style="display: block; margin: 0 auto">

- Now you can watch detail of the Pastry API.
<img src="/assets//images/17.png" width="70%" height="70%" style="display: block; margin: 0 auto">

- Click on Definition tab and you can watch Swagger module now you can try out API directly on Red Hat Developer Hub.
<img src="/assets/images/18.png" width="70%" height="70%" style="display: block; margin: 0 auto">


You can add more APIs on Microcks and the plugin discover and listed on Red Hat Developer Hub.