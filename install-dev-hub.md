### Installation Red Hat Developer Hub on Red Hat Developer Sandbox

---

#### Create a Red Hat Developer Account.

To create a new Red Hat Developer Sandbox account please visit next [Red Hat Developer Article](https://developers.redhat.com/articles/2023/03/30/how-access-developer-sandbox-red-hat-openshift)

---

- Open Red Hat Developer Sandbox Terminal.

![Terminal](/images/1.png "Open Console")

---
> Configuration files are available on my [Gist Repo](https://gist.githubusercontent.com/mikeintoch)

### Install Microcks

On Terminal run following commands.

```
oc create -f https://gist.githubusercontent.com/mikeintoch/895106c7c1005b34a157c7ae33835b83/raw/
```

As an output you install Microcks with non-persistent configuration.

![Microcks Pod](/images/2.png "Microcks Pod")

---

### Install Red Hat Developer Hub using Helm Chart

> Configuration files are available on my [Gist Repo](https://gist.githubusercontent.com/mikeintoch)

On Terminal run following commands.

- Download helm configuration file for Red Hat Developer Hub.

```
curl https://gist.githubusercontent.com/mikeintoch/185bf33e9187164163adfc79093fee1a/raw > rhdh.yaml

```
### Remove default authorization configuration
Configuration file has a line to disable Authentication

```
...
          dangerouslyDisableDefaultAuthPolicy: true
...
```

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

![URL DOMAIN](/images/3.png "URL Domain")

apps.{your_sandbox_url}.openshiftapps.com is the value on your sandbox url, see the URL bar at the top of above figure to see where to find this information in your Developer Sandbox URL) and run following command.
```
sed -i -e "s/CLUSTER_DOMAIN/apps.{your_sandbox_url}.openshiftapps.com/g" rhdh.yaml
```
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

![Devhub](/images/4.png "DevHub")

## Open Developer Hub in a browser.

![Devhub](/images/5.png "DevHub")

Default Authentication is disable, therefore you can enter Developer Hub using Guest User.

![Devhub](/images/6.png "DevHub")


## Create Mock API's with Microcks

- Open Microcks console
![Microcks](/images/7.png "Microcks")

- Click on **Microcks Hub**
![Microcks](/images/8.png "Microcks")

- Click on **MicrocksIO Samples API**
![Microcks](/images/9.png "Microcks")

- Click on **Pastry API**
![Microcks](/images/10.png "Microcks")

- Click on **Install**
![Microcks](/images/11.png "Microcks")

- Click on **Direct Import**
![Microcks](/images/12.png "Microcks")

- API is successfully installed
![Microcks](/images/13.png "Microcks")

- Click on **APIs|Services** and you can see API Pastry listed.
![Microcks](/images/14.png "Microcks")


## Validate integration with Microcks and Red Hat Developer Hub

Now Developer Hub is able to show APIs you create on Microcks through plugin previously installed.

- Open your Red Hat Developer Hub console in the left menu click on APIs
![DevHub](/images/15.png "DevHub")

- RHDH shows APIs listed then click on the name of the API.
![DevHub](/images/16.png "DevHub")

- Now you can watch detail of the Pastry API.
![DevHub](/images/17.png "DevHub")

- Click on Definition tab and you can watch Swagger module now you can try out API directly on Red Hat Developer Hub.
![DevHub](/images/18.png "DevHub")


You can add more APIs on Microcks and the plugin discover and listed on Red Hat Developer Hub.