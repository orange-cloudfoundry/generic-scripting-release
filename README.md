# BOSH Release for generic-scripting

## Usage

To use this bosh release, first upload it to your bosh:

```
bosh target BOSH_HOST
git clone https://github.com/cloudfoundry-community/generic-scripting-boshrelease.git
cd generic-scripting-boshrelease
bosh upload release releases/generic-scripting-1.yml
```

For [bosh-lite](https://github.com/cloudfoundry/bosh-lite), you can quickly create a deployment manifest & deploy a cluster:

```
templates/make_manifest warden
bosh -n deploy
```

For AWS EC2, create a single VM:

```
templates/make_manifest aws-ec2
bosh -n deploy
```

### Override security groups

For AWS & Openstack, the default deployment assumes there is a `default` security group. If you wish to use a different security group(s) then you can pass in additional configuration when running `make_manifest` above.

Create a file `my-networking.yml`:

``` yaml
---
networks:
  - name: generic-scripting1
    type: dynamic
    cloud_properties:
      security_groups:
        - generic-scripting
```

Where `- generic-scripting` means you wish to use an existing security group called `generic-scripting`.

You now suffix this file path to the `make_manifest` command:

```
templates/make_manifest openstack-nova my-networking.yml
bosh -n deploy
```

### Development

#### On local workstation
As a developer of this release, create new releases and upload them:

```
bosh create release --force && bosh -n upload release
```

#### Using GitHub Action

Look at https://github.com/orange-cloudfoundry/generic-scripting-release/actions/workflows/on-commits.yml to find the
dev release associated to your commit.

### Final releases

TODO: document
#### On local workstation and GitHub

1. create a tag locally
2. push it
3. observe the result on https://github.com/orange-cloudfoundry/generic-scripting-release/actions/workflows/on-tags.yml
4. download artefact at https://github.com/orange-cloudfoundry/generic-scripting-release/releases

#### On local workstation

To share final releases:

```
bosh create release --final
```

By default the version number will be bumped to the next major number. You can specify alternate versions:


```
bosh create release --final --version 2.1
```

After the first release you need to contact [Dmitriy Kalinin](mailto://dkalinin@pivotal.io) to request your project is added to https://bosh.io/releases (as mentioned in README above).
