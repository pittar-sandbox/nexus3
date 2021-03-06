# Nexus 3 on OpenShift 4 using Red Hat Univeral Base Image (UBI)

If you want to run Nexus 3 on OpenShift 4 without having to grant it escalated privileges (always best to avoid this) you have two options:

## UBI-Based Nexus 3 from Red Hat Registry

If you already have access to the Red Hat Registry (and if you are using OpenShift, you should be good to go), you can [pull it directly from the Red Hat Registry](https://catalog.redhat.com/software/containers/sonatype/nexus-repository-manager/594c281c1fbe9847af657690).

Make sure you click on the **Get this image** tab to get instructions on how to create a *registry token* or how to use your Red Hat credentaials to pull the image.

## Build UBI-Based Nexus 3 from a Dockerfile

Sonatype also provides a special `Dockerfile.rh.ubi` that can be used to build the image yourself with:
* docker
* [Buildah]()
* [Podman}()

Or, build directly on OpenShift with a Docker build.

The **base** directory of this repository has everything you need to build and deploy Nexus 3 on your cluster.  It inculdes:
* `BuildConfig` that uses the UBI-based Dockerfile from the `3.30.0` tag of the Nexus GitHub repository
* `ImageStream` for the Nexus 3 image
* `Deployment` to run Nexus 3
* `Service` for port `8081`
* `Route` with *edge* TLS termination
* `PersistentVolumeClaim` for Nexus storage.  Update this to the size that's appropriate for you.

To get this going on your cluster, login with `oc` and run:

```
$ oc new-project nexus3
$ oc apply -k /base
```

It will take a few minutes to build the Nexus image.  Once Nexus itself starts up, it will take at least 2-3 mintutes to start as it completes some 1st time installation tasks.

## Logging Into Nexus 3

The default **username** for Nexus 3 is `admin`.  The password, however, is generated.

To find your default generated password, run the following command from your Nexus 3 projects:

```
// Find the Nexus pod name.
$ oc get pods 
$ oc exec <nexus pod name> -- cat /nexus-data/admin.password
```

```


