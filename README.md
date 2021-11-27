# macchina.io REMOTE - Customize Web User Interface

## About macchina.io REMOTE

[macchina.io REMOTE](https://macchina.io/remote) provides secure remote access to connected devices
via HTTP or other TCP-based protocols and applications such as secure shell (SSH) or
Virtual Network Computing (VNC). With macchina.io REMOTE, any network-connected device
running the macchina.io REMOTE Device Agent software (*WebTunnelAgent*)
can be securely accessed remotely over the internet from browsers, mobile apps, desktop,
server or cloud applications.

This even works if the device is behind a NAT router, firewall or proxy server.
The device becomes just another host on the internet, addressable via its own URL and
protected by the macchina.io REMOTE server against unauthorized or malicious access.
macchina.io REMOTE is a great solution for secure remote support and maintenance,
as well as for providing secure remote access to devices for end-users via web or
mobile apps.

Visit [macchina.io/remote](https://macchina.io/remote) to learn more and to register for a free account.
Specifically, see the [Getting Started](https://macchina.io/remote_signup.html) page and the
[Frequently Asked Questions](https://macchina.io/remote_faq.html) for
information on how to use the macchina.io REMOTE device agent.

There is also a [blog post](https://macchina.io/blog/?p=257) showing step-by-step instructions to connect a Raspberry Pi.


## About This Repository

This repository contains a documentation and sample configuration files to customize
the web user interface of macchina.io REMOTE server deployed in a Docker
container.


## Prerequisites

You should already be familiar with setting up a Docker container for the macchina.io
REMOTE server, as described in the following repositories:

  - https://github.com/my-devices/meta-reflector-docker
  - https://github.com/my-devices/meta-reflector-aws

The changes described in this repository must be applied to one
of the above repositories.

## Customizing the Web User Interface

The web user interface of macchina.io REMOTE server can be customized
by providing a so-called *extension bundle*, which will:

  - provide a custom stylesheet
  - provide a custom logo
  - customize certain other aspects of the web user interface by
    overriding default configuration settings
  - provide user interface text in a different language

Please see the macchina.io REMOTE Server Set-Up and Administration Guide,
section 5.7.4 - Changing the Style and Branding of the Default Web User Interface,
for detailed information about customizing the web user interface.

The key point is to create an extension bundle containing the necessary resources
and meta-data. Unlike the example in the manual, which creates the extension bundle
manually using a Zip utility, with Docker, the bundle can be created with the
*BundleCreator* utility (`bundle`) included in the Docker image. To do so, the
[`Dockerfile`](reflector/Dockerfile) must be extended to copy the bundle sources into the build image
and run the `bundle` utility to create the bundle and copy it to the
`/home/reflector/lib/bundles` directory, where it will be picked up by
the server.

The sources for the bundle have been added to the [`reflector`](reflector) directory
in the repository. The following files and directories have been added:

  - [`branding`](reflector/branding): directory containing all resources
  - [`branding/branding.bndlspec`](reflector/branding/branding.bndlspec): An XML file
    containing bundle metadata. The file can be left as is.
  - [`branding/bundle`](reflector/branding/bundle): directory containing the
    bundle resources (properties, stylesheet, images, ...).
  - [`branding/bundle/bundle.properties`](reflector/branding/bundle/bundle.properties):
    configuration properties, as well as text for the user interface. This can be
    changed as desired.
  - [`branding/bundle/css/customstyles.css`](reflector/branding/bundle/css/customstyles.css):
    custom stylesheet for web user interface. This can be changed as desired.
  - [`branding/bundle/images/custom-logo.png`](reflector/branding/bundle/images/custom-logo.png):
    custom logo (references from `customstyles.css`). This can be replaced with your
    own logo.

In addition, two commands have been added to the [`Dockerfile`](reflector/Dockerfile):

```
COPY branding /home/reflector/branding
RUN cd /home/reflector/branding && bundle -o/home/reflector/lib/bundles branding.bndlspec
```

The first command copies the extension bundle sources to the Docker image, and the
second command runs the `bundle` command to generate a bundle file
(`net.my-devices.reflector.custom-branding.bndl`) in the `/home/reflector/lib/bundles`
directory.
