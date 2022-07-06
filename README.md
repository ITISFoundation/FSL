# fsl

FSL is a comprehensive library of analysis tools for FMRI, MRI and DTI brain imaging data, created by the Analysis Group, FMRIB, Oxford, UK. See https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FSL for more information.

## Basic concept

An osparc-ui module consists of two services, a web proxy ```web``` and the actual application ```app```.  The ```web``` service acts only as a proxy for integration into the osparc platform. The ```app``` service consists of the X11 based GUI application that should be accessible via the osparc iframe and some additional supporting programs:
- [supervisord](http://supervisord.org/): to control the processes in the container
- [xtigervnc](https://tigervnc.org/): VNC client/server application with embedded X-server
- [novnc](https://novnc.com/info.html): opensource javascript vnc client
- [openbox](http://openbox.org/wiki/Main_Page): Minimal window manager

## Usage


Build the module:
```console
$ make build
```
To run locally at and visit http://127.0.0.1:28080
```console
make run-local
```
To publish in local throw-away registry:
```console
make publish-local
```

## Workflow

1. The application should be installed via modification of the ```Dockerfile``` in ```fsl/app```
2. The  ```[program:app]``` section in ```supervisord.conf``` in ```fsl/app/config```  needs to be modified to accomodate to the correct command line for the program.

## Versioning

Two versions:

- integration version (e.g. ```fsl/VERSION_INTEGRATION```) is updated with ``make version-integration-*``
- service version (e.g. ```fsl/VERSION```) is updated with ``make version-service-*``

## CI/CD Integration
A template ci config file is created in```fsl/ci/.gitlab-ci.yml```)

### Gitlab

The required CI is already packaged.
To build and push to the internal registry you must add it to the [oSparc/docker-publisher-osparc-services](https://git.speag.com/oSparc/docker-publisher-osparc-services) repository.# FSL
