### Installation Red Hat Developer Hub on Red Hat Developer Sandbox

---

#### Create a Red Hat Developer Account.

To create a new Red Hat Developer Sandbox account please visit next [Red Hat Developer Article](https://developers.redhat.com/articles/2023/03/30/how-access-developer-sandbox-red-hat-openshift)

---
Red Hat Develoepr Sandbox brings you the tools to interact with your namespace.

- Open Red Hat Developer Sandbox Terminal.

<img src="/assets/images/1.png" width="50%" height="50%" style="display: block; margin: 0 auto">

### Install Red Hat Developer Hub using Helm Chart

On Terminal run following commands.

- Add Helm Repository.

```shell
helm repo add openshift-helm-charts https://charts.openshift.io/
```

- Download helm configuration file for Red Hat Developer Hub.

```shell
helm show values openshift-helm-charts/redhat-developer-hub > values.yaml
```
---

- Perform the required value changes.

<img src="/assets/images/3.png" width="50%" height="50%" style="display: block; margin: 0 auto">


apps.{your_sandbox_url}.openshiftapps.com is the value on your sandbox url, see the URL bar at the top of above figure to see where to find this information in your Developer Sandbox URL) and run following command.

```shell
sed -i -e 's/apps.example.com/apps.{your_sandbox_url}.openshiftapps.com/g' values.yaml
```
- Install RHDH chart using configuration file.

```shell
helm install -f values.yaml --version 1.2.1 redhat-developer-hub openshift-helm-charts/redhat-developer-hub
```

You can view Red Hat Developer Hub Pods Running.

<img src="/assets/images/4.png" width="50%" height="50%" style="display: block; margin: 0 auto">

Your application is now running with its default configuration.


### Disable Authentication.

- Execute in Developer Sandbox Terminal following commands

```shell
git clone https://github.com/mikeintoch/developer-hub.git

cd developer-hub

oc create -f ./assets/config/cm-disable-auth.yaml

```

This creates a new config map with following content

```yaml
kind: ConfigMap
apiVersion: v1
metadata:
  name: app-config-rhdh
data:
  app-config-rhdh.yaml: |
    backend:
        auth:
          dangerouslyDisableDefaultAuthPolicy: true
```

- Upgrade helm chart configuration adding new config map

```shell
helm upgrade --reuse-values redhat-developer-hub openshift-helm-charts/redhat-developer-hub \ 
--set "upstream.backstage.extraAppConfig[0].configMapRef=app-config-rhdh" \ 
--set "upstream.backstage.extraAppConfig[0].filename=app-config-rhdh.yaml"
```

## Open Developer Hub in a browser.

- For now, select the Route icon in the top-right of the roundel for the developer-hub application and the follwing screen will appear in a separate tab.

<img src="/assets/images/5.png" width="70%" height="70%" style="display: block; margin: 0 auto">

Default Authentication is disable, therefore you can enter Developer Hub using Guest User.

<img src="/assets/images/6.png" width="70%" height="70%" style="display: block; margin: 0 auto">

### Enjoy!