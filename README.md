# Catserver in Kubernetes
This repository contains simple kubernetes manifests that are needed in order to server cat images via a nginx deployment.

## Kubernetes Manifests
* `namespace.yml` contains the namespace `catserver`.
We genereated it using `kubectl create namespace catserver`

* `deployment.yml` contains the Deployment Manifest for the nginx webserver.
It mounts the two configmaps aswell a normal volume containing the images of the cats.

* `service.yml` contains the service exposing the pod via ClusterIP.
We can generate a simple service using the following command (this is what we did):
`kubectl create service clusterip catserver --tcp=80 -n catserver`

* `ingress.yml` contains the ingress definition exposing the service `catserver` on port 80.
We created our ingress with the command `kubectl create ingress catserver --rule="/*=catserver:80" -n catserver`

* `html-config.yml` contains the html file that is delivered to the user.

* `nginx-config.yml` contains the `nginx.conf` configuration file for nginx.
In this file we configure that the server accepts requests to the `/cats` path and delivers files from `/usr/share/nginx/html`

After applyinf all configuration we need to copy our images into the created PeristentVolume. See `kubectl describe pvc -n catserver` for the path where the PVC stores its data. Copy the images of the cats there.

## Image contribution
Thanks to Manja Vitolic (https://unsplash.com/de/fotos/gKXKBY-C-Dk, cat1.jpg) and Amber Kipp (https://unsplash.com/de/fotos/75715CVEJhI, cat2.jpg) for their contribution of their cat images to unsplash.
