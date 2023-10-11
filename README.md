# FSL - FMRIB Software Library

FSL is a comprehensive library of analysis tools for FMRI, MRI and DTI brain imaging data, created by the Analysis Group, FMRIB, Oxford, UK. See https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FSL for more information.

## Basic concept (for oSPARC UI Services)

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
## Test the application with some actual data
In a local deployment, you should be able to use the FSL GUI. You can try out different commands (by clicking on the respective button). For instance, you can try out the ```BET``` tools with the input file in (```validation/inputs/```).

## Workflow

1. The application should be installed via modification of the ```Dockerfile``` in ```fsl/app```
2. The  ```[program:app]``` section in ```supervisord.conf``` in ```fsl/app/config```  needs to be modified to accomodate to the correct command line for the program.

## Versioning
- To update service version use one of the commands ``make version-*``

## CI/CD Integration
A template ci config file is created in```fsl/.github/workflows/check-image.yml```. Then CI are monitored by the publisher repo on GitLab that takes care of publishing a new version.
