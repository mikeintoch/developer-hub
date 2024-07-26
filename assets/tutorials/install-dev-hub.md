### Installation Red Hat Developer Hub on Red Hat Developer Sandbox

---

#### Create a Red Hat Developer Account.

To create a new Red Hat Developer Sandbox account please visit next [Red Hat Developer Article](https://developers.redhat.com/articles/2023/03/30/how-access-developer-sandbox-red-hat-openshift)

---
Red Hat Develoepr Sandbox brings you the tools to interact with your namespace.

- Open Red Hat Developer Sandbox Terminal.

<img src="/images/1.png" width="50%" height="50%" style="display: block; margin: 0 auto">

---
> Configuration files are available on my [Gist Repo](https://gist.githubusercontent.com/mikeintoch)

---

### Install Microcks

On Terminal run following commands.

```
oc create -f https://gist.githubusercontent.com/mikeintoch/895106c7c1005b34a157c7ae33835b83/raw/
```

As an output you install Microcks with non-persistent configuration.

<img src="/images/2.png" width="50%" height="50%" style="display: block; margin: 0 auto">
---

### Install Red Hat Developer Hub using Helm Chart

> Configuration files are available on my [Gist Repo](https://gist.githubusercontent.com/mikeintoch)

On Terminal run following commands.

- Download helm configuration file for Red Hat Developer Hub.

```
curl https://gist.githubusercontent.com/mikeintoch/185bf33e9187164163adfc79093fee1a/raw > rhdh.yaml

```
---
### Remove default authorization configuration
Configuration file has a line to disable Authentication.

```
...
          dangerouslyDisableDefaultAuthPolicy: true
...
```
---

If you want to enable authentication you can follow [Github Configuration](/github-auth.md)

#### Modify values on configuration file

> Microcks Plugin configuration.
> ```
> ...
> dynamic:
>   includes:
>      - dynamic-plugins.default.yaml
>    plugins:
>     - integrity: >-
>          sha512-/Pre7TmeJkVgPrdfTiG0wcYpZ1KM14/AE8kSwUfVGtae4k4/lqaUfMnexnp75ujXHItm+iuSWvCRNhOvEw3PuA==
>        package: '@microcks/microcks-backstage-provider@0.0.5'
>        pluginConfig:
>          catalog:
>            providers:
>              microcksApiEntity:
>                dev:
>                  addOpenAPIServerUrl: true
>                 baseUrl: https://MICROCKS_URL
>                  ownerLabel: team
>                  schedule:
>                    frequency:
>                      minutes: 2
>                    timeout:
>                      minutes: 1
>                  serviceAccount: microcks-serviceaccount
>                 serviceAccountCredentials: ab54d3
>...
>```

Modify **MICROCKS_URL** value with following command:
```
sed -i -e "s/MICROCKS_URL/$(oc get route microcks-uber --output jsonpath={.spec.host})/g" rhdh.yaml
```

> Cluster Base domain
> 
>```
>...
> clusterRouterBase: CLUSTER_DOMAIN
>...
>```

Modify **CLUSTER_DOMAIN** value using your domain:

<img src="/images/3.png" width="50%" height="50%" style="display: block; margin: 0 auto">

apps.{your_sandbox_url}.openshiftapps.com is the value on your sandbox url, see the URL bar at the top of above figure to see where to find this information in your Developer Sandbox URL) and run following command.
```
sed -i -e "s/CLUSTER_DOMAIN/apps.{your_sandbox_url}.openshiftapps.com/g" rhdh.yaml
```
---

### Install Red Hat Developer Hub.

- Add Helm Repository.
```
helm repo add openshift-helm-charts https://charts.openshift.io/
```

- Install RHDH chart using configuration file.
```
helm install -f rhdh.yaml --version 1.2.1 redhat-developer-hub openshift-helm-charts/redhat-developer-hub
```

As a result you can view Red Hat Developer Hub Pods Running.

<img src="/images/4.png" width="50%" height="50%" style="display: block; margin: 0 auto">

## Open Developer Hub in a browser.

<img src="/images/5.png" width="70%" height="70%" style="display: block; margin: 0 auto">

Default Authentication is disable, therefore you can enter Developer Hub using Guest User.

<img src="/images/6.png" width="70%" height="70%" style="display: block; margin: 0 auto">

---

## Create Mock API's with Microcks

- Open Microcks console
<img src="/images/7.png" width="70%" height="70%" style="display: block; margin: 0 auto">

- Click on **Microcks Hub**
<img src="/images/8.png" width="30%" height="30%" style="display: block; margin: 0 auto">

- Click on **MicrocksIO Samples API**
<img src="/images/9.png" width="70%" height="70%" style="display: block; margin: 0 auto">

- Click on **Pastry API**
<img src="/images/10.png" width="70%" height="70%" style="display: block; margin: 0 auto">

- Click on **Install**
<img src="/images/11.png" width="70%" height="70%" style="display: block; margin: 0 auto">

- Click on **Direct Import**
<img src="/images/12.png" width="70%" height="70%" style="display: block; margin: 0 auto">

- API is successfully installed
<img src="/images/13.png" width="70%" height="70%" style="display: block; margin: 0 auto">

- Click on **APIs|Services** and you can see API Pastry listed.
<img src="/images/14.png" width="70%" height="70%" style="display: block; margin: 0 auto">

---

## Validate integration with Microcks and Red Hat Developer Hub

Now Developer Hub is able to show APIs you create on Microcks through plugin previously installed.

- Open your Red Hat Developer Hub console in the left menu click on APIs
<img src="/images/15.png" width="30%" height="30%" style="display: block; margin: 0 auto">

- RHDH shows APIs listed then click on the name of the API.
<img src="/images/16.png" width="70%" height="70%" style="display: block; margin: 0 auto">

- Now you can watch detail of the Pastry API.
<img src="/images/17.png" width="70%" height="70%" style="display: block; margin: 0 auto">

- Click on Definition tab and you can watch Swagger module now you can try out API directly on Red Hat Developer Hub.
<img src="/images/18.png" width="70%" height="70%" style="display: block; margin: 0 auto">


You can add more APIs on Microcks and the plugin discover and listed on Red Hat Developer Hub.