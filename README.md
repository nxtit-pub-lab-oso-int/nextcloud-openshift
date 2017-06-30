# Nextcloud for OpenShift 3

This repository contains an OpenShift 3 template to easily deploy Nextcloud on OpenShift.
With this template it's possible to run your own Nextcloud instance f.e. on [APPUiO](https://appuio.ch/).

## Installation

### 1 Deploy Database

```
oc -n openshift process mariadb-persistent -p MYSQL_DATABASE=nextcloud | oc -n MYNAMESPACE create -f -
```

### 2 Deploy Nextcloud

```
oc process -f https://raw.githubusercontent.com/tobru/nextcloud-openshift/master/nextcloud.yaml -p NEXTCLOUD_HOST=nextcloud.example.com | oc -n MYNAMESPACE create -f -
```

#### Template parameters

Execute the following command to get the available parameters:

```
oc process -f https://raw.githubusercontent.com/tobru/nextcloud-openshift/master/nextcloud.yaml --parameters
```

### 3 Configure Nextcloud

* Navigate to http://nextcloud.example.com
* Fill in the form and finish the installation. The DB credentials can be 
  found in the secret `mariadb`. In the Webconsole it can be found under
  `Resources -> Secrets -> mariadb -> Reveal Secret`
* Enable APCu in `config.php`. See also https://github.com/nextcloud/docker/issues/109

**Hints**

* You might want to enable TLS for your instance

## Backup

Still to be done:

* Database
* Files

## Notes

* Nextcloud Cronjob is called from a `CronJob` object every 15 minutes
* The Dockerfile just add the `nginx.conf` to the Alpine Nginx container

## Ideas

* Use sclorg Nginx instead of Alpine Nginx for better OpenShift compatibility
* Autoconfigure Nextcloud using `autoconfig.php`

## Contributions

Very welcome!

1. Fork it (https://github.com/tobru/nextcloud-openshift/fork)
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request
