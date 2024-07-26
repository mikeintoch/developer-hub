# Integrate Microcks with Red Hat Developer Hub

### Install Microcks

On Terminal run following commands.

```
oc create -f https://gist.githubusercontent.com/mikeintoch/895106c7c1005b34a157c7ae33835b83/raw/
```

As an output you install Microcks with non-persistent configuration.

<img src="/images/2.png" width="50%" height="50%" style="display: block; margin: 0 auto">
---

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