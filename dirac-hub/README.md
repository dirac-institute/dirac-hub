# EKS Helm Chart

This helm chart manages a deployment of JupyterHub on EKS with an EFS storage system. This chart is based on [Pangeo](https://github.com/pangeo-data/helm-chart), and adds two templates for storage management, which are managed by these exposed values:

```yaml
efs:
  # set efs storage on or off
  enabled: false
  # specify the folders that you would like to open a peristent volume claim for on the k8s cluster
  folders:
    # each folder has a corresponding persistent volume claim
  - pvcName:
    # and a sub-path from the root folder "/"
    subPath:
  # the id of the EFS system, e.g. fs1234
  efsId:
  # the sub-path within the filesystem to act as root "/"
  # for example, using
  # subPath: astro-hub
  # will set up a file-system under "/astro-hub"
  subPath:
  # the AWS region to use. Default = us-west-2
  region:

```

These values are used in the following template:

`template/storage.yaml`: Generates one persistent volume claim for each of the folders specified in `efs.folders`

`template/mount_busybox.yaml`: A Kubernetes Job that will create the folders in `efs.folders` on the EFS file system if they don't already exist. This avoids mounting non-existent directories on the filesystem.
