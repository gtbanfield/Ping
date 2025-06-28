# generic-repo-template
Ping Setup
### Ping 7.5 IDA Setup
```
All artifacts are in Nexus
/PING/Amster-7.5.1.zip
/PING/DS-7.5.2.zip
/PING/AM-7.5.1.zip

Folder Structures are as follows for the zip in Nexus.
amster
openam
opendj


All the setup's have been run and repackaged up as Zips and uploaded to Nexus.
- [x] For opendj all folder and binararies needed to be copied to samples/docker/ from the opendj root folder.
```

### Base Platform
```
There is a run step which curls the zip and then copies the docker files

First you have to build the core platform


- [x] Java-17-BASE (from: forgeops-extras/images/java-17)
      
- [x] Amster-BASE (image from Java-17-BASE)
---
- [x] AM-EMPTY 
- [x] AM-BASE
- [x] AM-CDK
---
DS-EMPTY (image from Java-17-BASE)

```

### ForgeOps
```
The images from above Base Platformbuilds are required for forgeops to build PING services.

---
- Ping/forgeops/docker/am:
AM (image tag taken from AM-CDK build)

- Ping/forgeops/docker/ds/ds-new:
DS (image tag taken from DS-EMPTY build)

- Ping/forgeops/docker/amster:
Amster (image tag taken from Amster-BASE build)


```

### Harness
```
Each build is handled via input sets
The contextpath point to the each Zips Dockerfile where harness is copying them to:
- opendj/samples/docker/
- amster/samples/docker/
- openam/samples/docker/images/am-empty/
- openam/samples/docker/images/am-base/
- openam/samples/docker/images/am-cdk/
```


docker/7.5/DS-Empty/Dockerfile /harness/opendj/samples/docker/

gtbanf1979/ping/java-17-base