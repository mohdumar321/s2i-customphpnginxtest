
UMAR DOCUMENTATION



BUILDS

The OpenShift Container Platform build system provides extensible support for build strategies that are based on selectable types specified in the build API. There are three primary build strategies available:


•	Docker build

•	Source-to-Image (S2I) build

•	Custom build

By default, Docker builds and S2I builds are supported.

The resulting object of a build depends on the builder used to create it. For Docker and S2I builds, the resulting objects are runnable images. For Custom builds, the resulting objects are whatever the builder image author has specified.

Docker Build

The Docker build strategy invokes the docker build command, and it therefore expects a repository with a Dockerfile and all required artifacts in it to produce a runnable image.

Source-to-Image (S2I) Build

Source-to-Image (S2I) is a tool for building reproducible, Docker-formatted container images. It produces ready-to-run images by injecting application source into a container image and assembling a new image. The new image incorporates the base image (the builder) and built source and is ready to use with the docker run command. 

https://blog.openshift.com/create-s2i-builder-image/

https://docs.openshift.org/latest/creating_images/guidelines.html

Custom Build

The Custom build strategy allows developers to define a specific builder image responsible for the entire build process. Using your own builder image allows you to customize your build process.
A Custom builder image is a plain Docker-formatted container image embedded with build process logic, for example for building RPMs or base images.

https://docs.openshift.com/container-platform/3.3/architecture/core_concepts/builds_and_image_streams.html#source-build

centos-release-scl-rh

https://www.softwarecollections.org/en/scls/?page=3

bcrypt is a password hashing function 

MKDIR -P

https://linux.die.net/man/1/mkdir

https://ss64.com/bash/chmod.html

The CentOSPlus repository contains a group of packages, all of which are updates. You probably do not want to enable CentOSPlus as a whole, but instead want to pick the exact packages that you want to use.


RPM command is used for installing, uninstalling, upgrading, querying, listing, and checking RPM packages on your Linux system.
RPM stands for Red Hat Package Manager.
With root privilege, you can use the rpm command with appropriate options to manage the RPM software packages.


http://superuser.com/questions/62141/how-to-move-all-files-from-current-directory-to-upper-directory

https://getcomposer.org/doc/00-intro.md





http://stackoverflow.com/questions/38256278/nginx-configuration-for-cakephp3-on-openshift






































































































































































PHP Docker images
=================

This repository contains the source for building various versions of
the PHP application as a reproducible Docker image using
[source-to-image](https://github.com/openshift/source-to-image).
Users can choose between RHEL and CentOS based builder images.
The resulting image can be run using [Docker](http://docker.io).

For more information about using these images with OpenShift, please see the
official [OpenShift Documentation](https://docs.openshift.org/latest/using_images/s2i_images/php.html).

Versions
---------------
PHP versions currently supported are:
* php-5.6
* php-7.0

RHEL versions currently supported are:
* RHEL7

CentOS versions currently supported are:
* CentOS7


Installation
---------------
To build a PHP image, choose either the CentOS or RHEL based image:
*  **RHEL based image**

    To build a RHEL based PHP image, you need to run the build on a properly
    subscribed RHEL machine.

    ```
    $ git clone https://github.com/sclorg/s2i-php-container.git
    $ cd s2i-php-container
    $ make build TARGET=rhel7 VERSION=7.0
    ```

*  **CentOS based image**
    ```
    $ git clone https://github.com/sclorg/s2i-php-container.git
    $ cd s2i-php-container
    $ make build TARGET=centos7 VERSION=7.0
    ```

Alternatively, you can pull the CentOS image from Docker Hub via:

    $ docker pull centos/php-70-centos7

**Notice: By omitting the `VERSION` parameter, the build/test action will be performed
on all the supported versions of PHP.**


Usage
---------------------------------

For information about usage of Dockerfile for PHP 7.0,
see [usage documentation](7.0/README.md).

For information about usage of Dockerfile for PHP 5.6,
see [usage documentation](5.6/README.md).

Test
---------------------
This repository also provides a [S2I](https://github.com/openshift/source-to-image) test framework,
which launches tests to check functionality of a simple PHP application built on top of the s2i-php image.

Users can choose between testing a PHP test application based on a RHEL or CentOS image.

*  **RHEL based image**

    This image is not available as a trusted build in [Docker Index](https://index.docker.io).

    To test a RHEL7 based PHP-5.5 image, you need to run the test on a properly
    subscribed RHEL machine.

    ```
    $ cd s2i-php-container
    $ make test TARGET=rhel7 VERSION=7.0
    ```

*  **CentOS based image**

    ```
    $ cd s2i-php-container
    $ make test TARGET=centos7 VERSION=7.0
    ```

**Notice: By omitting the `VERSION` parameter, the build/test action will be performed
on all the supported versions of PHP.**


Repository organization
------------------------
* **`<php-version>`**

    Dockerfile and scripts to build container images from.

* **`hack/`**

    Folder containing scripts which are responsible for the build and test actions performed by the `Makefile`.

Image name structure
------------------------

1. Platform name (lowercase) - php
2. Platform version(without dots) - 70
3. Base builder image - centos7/rhel7

Examples: `php-70-centos7`, `php-70-rhel7`

