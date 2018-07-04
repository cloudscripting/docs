# Basic Configs

The above two units display the outer side of a JPS usage and now let’s have a closer look at the inner side - a code of a package with all required configurations.

The JPS manifest is a file with <b>*.json*</b> extension, which contains an appropriate code written in JSON format. This manifest file includes the links to the web only dependencies. This file can be named as you require. 

The code should contain a set of strings needed for a successful installation of an application. The basis of the code is represented by the following string:
@@@
```yaml
type: string
name: any required name
```
``` json
{
    "type": "string",
    "name": "any required name"
}
```
@@!

- *type*
    - `install` - creating at least one environment     
    - `update` - extension, installing in one of existing environments    
- *name* - JPS custom name           

This is a mandatory body part of the application package, which includes the information about JPS name and the type of the application installation (the <b>*'install'*</b> mode initiates a new environment creation required for a deployment, the <b>*'update'*</b> mode performs actions on the existing environment).
This basic string should be extended with the settings required by the application you are packing. The following configuration details are included beside the <b>*'type': " "*</b> parameter:

<h2>Manifest Overview</h2>

There is a set of available parameters to define a manifest installation behaviour, custom description and design, application icons and success texts etc.

**Basic Template**
@@@
```yaml
type: string
version: string
name: string
logo: string
description: string
homepage: string
categories: array
baseUrl: string
settings: object
targetRegions: object
nodeGroupAlias: object
nodes: array
engine: string
region: string
ssl: boolean
ha: boolean
displayName: string
appVersion: string
onInstall: object/string
startPage: string
actions: array
addons: array
success: object/string
...: object
```
``` json
{
  "type": "string",
  "version": "string",
  "name": "string",
  "logo": "string",
  "description": "object/string",
  "homepage": "string",
  "categories": "array",
  "baseUrl": "string",
  "settings": "object",
  "targetRegions" : "object",
  "nodeGroupAlias": "object",
  "nodes": "array",
  "engine": "string",
  "region": "string",
  "ssl": "boolean",
  "ha": "boolean",
  "displayName": "string",
  "appVersion": "string",
  "onInstall": "object/array",
  "startPage": "string",
  "actions": "array",
  "addons": "array",
  "success": "object/string",
  "...": "object"
}
```
@@!

- `type` *[optional]* - type of the application installation. Available values are **install** and **update**. More details described above. 
- `version` - *[optional]* - JPS type supported by the Jelastic Platform. See the <a href="/jelastic-cs-correspondence/" target="_blank">correspondence between version</a> page.
- `name` *[required]* - JPS custom name
- `logo` *[optional]* - JPS image that will be displayed within custom add-ons
- `description` - text string that describes a template. This section should always follow the template format version section.
- `homepage` *[optional]* - link to any external aplication source
- `categories` - categories available for manifests filtering                                                                        
- `baseUrl` *[optional]* - custom <a href="#relative-links" target="_blank">relative links</a>                                       
- `settings` *[optional]* - custom form with <a href="/creating-manifest/visual-settings/" target="_blank">predefined user input elements</a>
- `targetRegions` *[optional]* - filtering available regions on Jelastic platform. Option will be used only with **type** `install`
    - `type` *[optional]* [array] - region's virtualization types
    - `name` *[optional]* [string] - text or JavaScript RegExp argument to filtering region's by name
- `region` *[optional]* - region, where an environment will be installed. Option will be used only with **type** `install`.
`targetRegions` has has a higher priority than `region`. So in case when both of options have been set regions will be filtered according to the `targetRegions` rules.
- `nodeGroupAlias` *[optional]* - an ability to set aliases for existed in environments *nodeGroup*
- `nodes` - an array to describe information about nodes for an installation. Option will be used only with **type** `install`.
- `engine` *[optional]* - engine <a href="/creating-manifest/selecting-containers/#engine-versions" target="_blank">version</a>, by **default** `java6`
- `ssl` *[optional]* - Jelastic SSL status for an environment, by **default** `false`. Parameter is available only with **type** `install` mode.
- `ha` *[optional]* - high availability for Java stacks, by **default** `false`. Parameter is available only with **type** `install` mode.
- `displayName` *[optional]* - display name for an environment. Required option for **type** `install`.
- `appVersion` *[optional]* - custom version of an application
- `onInstall` *[optional]* - <a href="/creating-manifest/events/#oninstall" target="_blank">event</a> that is an entry point for actions execution
- `startPage` *[optional]* - path to be opened via the **Open in browser** button through a successful installation message
- `actions` *[optional]* - objects to describe all <a href="/creating-manifest/actions/#custom-actions" target="_blank">custom actions</a>
- `addons` *[optional]* - includes JPS manifests with the **type** `update` as a new JPS installation
- `success` *[optional]* - success text that will be sent via email and will be displayed at the dashboard after installation. There is an ability to use Markdown syntax. More details [here](/creating-manifest/visual-settings/#success-text-customization).
- "..." - the list of <a href="/creating-manifest/events/" target="_blank">events</a> can be predefined before manifest is installed. More details 

##Environment Installation

The environment can be installed in case when the `type` parameter is set to **install**. Then the set of nodes with their parameters should be defined also.

###Nodes Definition

The list of available parameters are:

- `nodeType` *[required]* - the defined node type. The list of available stacks are <a href="/creating-manifest/selecting-containers/#supported-stacks" target="_blank">here</a>. 
- `cloudlets` *[optional]* - a number of dynamic cloudlets. The default value is 0. `flexible` is an alias. 
- `fixedCloudlets` *[optional]* - a mount of fixed cloudlets. The default value is 1.
- `count` *[optional]* - a mount of nodes in one group. The default value is 1.
- `nodeGroup` *[optional]* - the defined node layer. A docker-based containers can be predefined in any custom node group.
- `displayName` *[optional]* - node's display name (i.e. <a href="https://docs.jelastic.com/environment-aliases" target="_blank">alias</a>)                                         
- `extip` *[optional]* - attaching public IP address to a container. The default value is *'false'*.
- `addons` *[optional]* - a list of addons, which will be installed in current `nodeGroup`. Addons will be installed after environment installation and `onInstall` action will be finished. [More details here](/creating-manifest/addons/)
- `tag` *[optional]* - an image tag for `dokerized` Jelastic templates with `nodeType` parameter. Full list of supported tag [here](/creating-manifest/selecting-containers/#dokerized-template-tags).

The following parameters are available for Docker nodes only:   
                       
- `image` *[optional]* - name and tag of Docker image                            
- `links` *[optional]* - Docker links                         
    - `sourceNodeGroup` - source node to be linked with a current node                                
    - `alias` - prefix alias for linked variables                         
- `env` *[optional]* - Docker environment variables                        
- `volumes` *[optional]* - Docker node volumes               
- `volumeMounts` *[optional]* - Docker external volumes mounts                             
- `cmd` *[optional]* - Docker run configs                            
- `entrypoint` *[optional]* - Docker entry points  
- `startService` *[optional]* - defines whether to run defined service or not. By default `false`

<!--##STAR TNEW### StartService issue description-->
###startService Parameter

The flag `startService` works only for custom dockers and for dockerized templates, and, accordingly, does not affect the cartridges and legacy native templates.

When creating a topology, scripting always passes the flag `startService=false` to all AddNode methods, which allows us to speed up the creation and avoid problems with the inaccessibility of some nodes during the launch of services.
Further, if in the topology description, the startService flag is not false, scripting calls in parallel the ExecDockerRunCmd methods for all the created containers via the AddNode method.

При создании топологии, скриптинг на все методы AddNode всегда передает флаг startService=false, что позволяет нам ускорить создание и избежать проблем с недоступностью каких-то нод в процессе запуска сервисов.
Далее, если в описании топологии, флаг startService не равен false, скриптинг параллельно вызывает методы ExecDockerRunCmd для всех созданных контейнеров через метод AddNode.
 
Во время вызова ExecDockerRunCmd, JelCore вызывает команду jem docker run, которая запускает сервис и добавляет его в автозагрузку (jem docker run вызывается еще в методе AddNode, если не был передан флаг startService=false).

<!--##END NEW### StartService issue description-->

<!--##Docker Actions-->
###Nodes Actions

Specific Cloud Scripting actions for Docker containers include operations of *volumes*, *links* and *environment variables* management.
<br>

There are three available parameters to set Docker volumes:  

- *volumes* - list of volume paths   
- *volumeMounts* - mount configurations  
- *volumesFrom* - list of nodes the volumes are imported from    

All of the fields are set within the Docker object:
@@@
```yaml
type: install
name: docker volumes

nodes:
  nodeGroup: sqldb
  image: centos:7
  volumes: []
  volumeMount: {}
  volumesFrom: []
```
``` json
{
  "type": "install",
  "name": "docker volumes",
  "nodes": [
    {
      "nodeGroup": "sqldb",
      "image": "centos:7",
      "volumes": [],
      "volumeMounts": {},
      "volumesFrom": []
    }
  ]
}
```
@@!

**Volumes**<br>
This field represents a string array:
@@@
```yaml
- volumes:
  - /external
  - /data
  - /master
  - /local
```
``` json
[
  {
    "volumes": [
      "/external",
      "/data",
      "/master",
      "/local"
    ]
  }
]
```
@@!

**VolumeMounts**<br>
This parameter is an object. It can be set like within the example below:
@@@
```yaml
volumeMounts:
  /example-path:
    sourcePath: ''
    sourceNodeId: 0
    sourceNodeGroup: ''
    sourceHost: ''
    readOnly: true
```
``` json
{
  "volumeMounts": {
    "/example-path": {
      "sourcePath": "",
      "sourceNodeId": 0,
      "sourceNodeGroup": "",
      "sourceHost": "",
      "readOnly": true
    }
  }
}
```
@@!
Here:  

- `/example-path` - path to place the volume at a target node  
- `sourcePath` *[optional]* - default value that repeats volume path (*/example-path* in our sample)
- `sourceNodeId` -  node identifier the volume should be mounted from (optional, in case of the `sourceNodeGroup` parameter using)       
- `sourceHost` *[optional]* - parameter for <a href="https://docs.jelastic.com/configure-external-nfs-server" target="_blank">external mounts</a> usage
- `readOnly` - defines write data permissions at source node, the default value is `false`   
- `sourceNodeGroup` - any available *nodeGroup* within a source environment (ignored if the `sourceNodeId` parameter is specified). The list of mounted volumes is defined by a master node.    

In case not all source node volumes are required to be mounted, the particular ones can be specified:
@@@
```yaml
- sourceNodeGroup: storage
  volumes: 
    - /master
    - /local
```
``` json
[
  {
    "sourceNodeGroup": "storage",
    "volumes": [
      "/master",
      "/local"
    ]
  }
]
```
@@!

<h4>VolumeMounts examples</h4>
 
**Master Node Mount:**
Samples to mount a particular volume by exact node identifier & path (*/master*) and to mount all volumes from the layer master node by *nodeGroup* (*/master-1*)
@@@
```yaml
volumeMounts:
  /master:
    sourcePath: /master
    sourceNodeId: 81725
    readOnly: true
  /master-1:
   sourceNodeGroup: current-node-group
```
``` json
{
  "volumeMounts": {
    "/master": {
      "sourcePath": "/master",
      "sourceNodeId": 81725,
      "readOnly": true
    },
    "/master-1": {
      "sourceNodeGroup": "current-node-group"
    }
  }
}
```
@@!

Here, *sourcePath* and *readOnly* parameters are optional.

**Mount Data Container:**
<br>
Samples to mount all volumes from a particular node by exact node identifier & path (*/node*) and to mount master node volumes by *nodeGroup* type (*/data*)
@@@
```yaml
volumeMounts:
  /node:
    sourceNodeId: 45
  /data:
    sourceNodeGroup: storage
```
``` json
{
  "volumeMounts": {
    "/node": {
      "sourceNodeId": 45
    },
    "/data": {
      "sourceNodeGroup": "storage"
    }
  }
}
```
@@!

**External Server Mounts:**
<br>
Sample to mount a volume (*/external*) from external server by indicating its host (`sourceHost`), path (`sourcePath`) and access permissions (`readOnly`).
@@@
```yaml
volumeMounts:
  /external:
    sourceHost: external.com
    sourcePath: /remote-path
    readOnly: true
```
``` json
{
  "volumeMounts": {
    "/external": {
      "sourceHost": "external.com",
      "sourcePath": "/remote-path",
      "readOnly": true
    }
  }
}
```
@@!
**Short Set for External Server:**
<br>
Sample to mount a number of volumes from external server by specifying the required parameters (i.e. volume path, `sourceHost`, `sourcePath`, access permissions) for each of them within one string.     
@@@
```yaml
volumeMounts:
  /ext-domain: aws.com
  /ext-domain/ro: aws.com;ro
  /ext-domain/path: aws.com:/121233
  /ext-domain/path/ro: aws.com:/121233:ro
```
``` json
{
  "volumeMounts": {
    "/ext-domain": "aws.com",
    "/ext-domain/ro": "aws.com:ro",
    "/ext-domain/path": "aws.com:/121233",
    "/ext-domain/path/ro": "aws.com:/121233:ro"
  }
}
```
@@!

Here, "*ro*" stands for *readOnly* permissions.

<!--
##volumesFrom

`volumesFrom` is an list object.    
There are two ways to select the volume source container:
``` json
[
  {
    "sourceNodeId": "49",
    "readOnly": true
  },{
    "sourceNodeGroup": "storage",
    "readOnly": true
  }
]
```

In case to import not full source node volumes list You can set like below:
``` json
[
  {
    "sourceNodeGroup": "storage",
    "volumes": [
      "/master",
      "/local"
    ]
  }
]
```

Simple set examples above:
``` json
[
  49,
  "storage",
  "storage:ro"
]
```
where:   
- *49* - like { sourceNodeId : 49, readOnly : false }  
- *"storage"* - like { sourceNodeGroup : "storage", readOnly : false }  
- *"storage:ro"* - like { sourceNodeGroup : "storage", readOnly : true }
-->

#### Environment Variables

Docker environment <a href="https://docs.jelastic.com/docker-variables" target="_blank">variable</a> is an optional topology object. The *env* instruction allows to set the required environment variables to specified values. 
@@@
```yaml
type: install
name: Environment variables

nodes:
  - nodeGroup: cp
    image: wordpress:latest
    env:
      WORDPRESS_VERSION: 4.6.1
      PHP_INI_DIR: /usr/local/etc/php
```
``` json
{
  "type": "install",
  "name": "Environment variables",
  "nodes": [
    {
      "nodeGroup": "cp",
      "image": "wordpress:latest",
      "env": {
        "WORDPRESS_VERSION": "4.6.1",
        "PHP_INI_DIR": "/usr/local/etc/php"
      }
    }
  ]
}
```
@@!

Environment variables can manage to control nodes availability from outside to the platform. <a href="https://docs.jelastic.com/setting-custom-firewall" target="_blank">Jelastic Container Firewall</a> feature was implemented in Jelastic version 5.4 and new firewall rules can be set during creating new environment.<br>
 The reserved environment variable for this option is - **JELASTIC_PORTS**. This parameter defines which ports will be added in *inbound* rules. All rules in this case will be added for both protocols (**TCP/UDP**).

@@@
```yaml
jpsType: install
name: JELASTIC_PORTS env variable
nodes:
  nodeType: apache2
  nodeGroup: cp
  env:
    JELASTIC_PORTS: 3306, 33061, 33062
```
```json
{
  "jpsType": "install",
  "name": "JELASTIC_PORTS env variable",
  "nodes": {
    "nodeType": "apache2",
    "nodeGroup": "cp",
    "env": {
      "JELASTIC_PORTS": "3306, 33061, 33062"
    }
  }
}
```
@@!
All ports for output traffic are opened by default.

Another one reserved environment variables is **ON_ENV_INSTALL**. This variable is responsible for executing new JPS installation after new nodeGroup (layer of nodes) has been created.<br>
This variable for **nodeGroup** can be set in JPS or via dashboard. More info about Docker configuration is Jelastic dashbboard <a href="https://docs.jelastic.com/docker-configuration" target="_blank">here</a>.

!!! note
    > By default in manifest from the **ON_ENV_INSTALL** variable *\${settings.nodeGroup}* placeholder is defined. It will be a nodeGroup value where this manifest is executed.

**ON_ENV_INSTALL** can consists of manifest URL (string) or an object.<br>
URL is a external link for manifest with any *type* - `install` or `update`. An object could has two options:

 - `jps` - link, source of external manifest
 - `settings` - a list of any parameters which will be defined in external manifest in *\${settings.\*}* scope.

In the first example **ON_ENV_INSTALL** is defined like simple URL:
@@@
```yaml
type: install
name: ON ENV INSTALL
nodes:
  nodeType: nginxphp
  env:
    ON_ENV_INSTALL: http://example.com/manifest.jps
```
```json
{
  "type": "install",
  "name": "ON ENV INSTALL",
  "nodes": {
    "nodeType": "nginxphp",
    "env": {
      "ON_ENV_INSTALL": "http://example.com/manifest.jps"
    }
  }
}
```
@@!

Another one example displays an ability to set any custom options which can be used in executed manifest from variable:

@@@
```yaml
type: install
name: ON ENV INSTALL
nodes:
  nodeType: nginxphp
  env:
    ON_ENV_INSTALL:
      jps: http://example.com/manifest.jps
      settings:
        customSetting: mySetting
```
```json
{
  "type": "install",
  "name": "ON ENV INSTALL",
  "nodes": {
    "nodeType": "nginxphp",
    "env": {
      "ON_ENV_INSTALL": {
        "jps": "http://example.com/manifest.jps",
        "settings": {
          "customSetting": "mySetting"
        }
      }
    }
  }
}
```
@@!
In the example above in *manifest.jps* a **\${settings.customSetting}** placeholder is available with value *mySetting*.
Any number of custom parameters in *settings* can be set.

#### Links

Docker <a href="https://docs.jelastic.com/docker-links" target="_blank">links</a> option allows to set up interaction between Docker containers, without having to expose internal ports to the outside world.
<br>

The example below illustrates the way to link *sql* and *memcached* nodes to *cp* container.
@@@
```yaml
- image: wordpress:latest
  links:
    - db:DB
    - memcached:MEMCACHED
  cloudlets: 8
  nodeGroup: cp
  displayName: AppServer

- image: mysql5:latest
  cloudlets: 8
  nodeGroup: db
  displayName: database

- image: memcached:latest
  cloudlets: 4
  nodeGroup: memcached
  displayName: Memcached
```
``` json
[
  {
    "image": "wordpress:latest",
    "links": [
      "db:DB",
      "memcached:MEMCACHED"
    ],
    "cloudlets": 8,
    "nodeGroup": "cp",
    "displayName": "AppServer"
  },
  {
    "image": "mysql5:latest",
    "cloudlets": 8,
    "nodeGroup": "db",
    "displayName": "Database"
  },
  {
    "image": "memcached:latest",
    "cloudlets": 4,
    "nodeGroup": "memcached",
    "displayName": "Memcached"
  }
]
```
@@!
where:

- `links` - object that defines nodes to be linked to *cp* node by their *nodeGroup* and these links names            
- `db` - MYSQL server `nodeGroup` (environment layer)  
- `memcached` - Memcached server `nodeGroup` (environment layer)   

As a result, all the environment variables within *db* and *memcached* nodes will be also available at *cp* container.  
 
Here, environment variables of linked nodes will have the names, predefined within the `links` array.     
For example:  

- variable *MYSQL_ROOT_PASSWORD* from *sql* node is *DB_MYSQL_ROOT_PASSWORD* in *cp* node   
- variable *IP_ADDRESS* from *memcached* node is *MEMCACHED_IP_ADDRESS* in *cp* node

###Entry Points
There is an ability to set custom entry points - the button *Open in Browser*, which can be clicked when JPS with type `install` is installed.
![open-in-browser.png](/img/open-in-browser.png)

Entry Points can be set in `startPage` option. The default `startPage` value is an installed environment URL (even it hasn't been defined).
Entry Points can include any general placeholders - which have been defined during enviroment installation.

For example:
@@@
```yaml
type: install
baseUrl: https://docs.cloudscripting.com/
nodes:
  nodeType: apache
  cloudlets: 8
startPage: ${baseUrl}creating-manifest/basic-configs/
```
```json
{
  "type": "install",
  "baseUrl": "https://docs.cloudscripting.com/",
  "nodes": {
    "nodeType": "apache",
    "cloudlets": 8
  },
  "startPage": "${baseUrl}creating-manifest/basic-configs/"
}
```
@@!

The case where any custom directory of created environment can be opened in *Open in Browser* button:
@@@
```yaml
type: install
nodes:
  nodeType: apache
  cloudlets: 8
startPage: ${env.url}customDirectory/
```
```json
{
  "type": "install",
  "nodes": {
    "nodeType": "apache",
    "cloudlets": 8
  },
  "startPage": "${env.url}customDirectory/"
}
```
@@!

##Relative Links

The relative links functionality is intended to specify the JPS file’s base URL, in relation to which the subsequent links can be set throughout the manifest. This source destination (URL) can point either to the text of the file or its raw code. Therefore, it is passed in the manifest through the <b>*baseUrl*</b> parameter or specified while <a href="https://docs.jelastic.com/environment-export-import" target="_blank">importing</a> a corresponding JPS file via the Jelastic dashboard.          

!!! note
    > The *baseUrl* value declared within the manifest has higher priority than installation via URL (i.e. <a href="https://docs.jelastic.com/environment-export-import" target="_blank">Import</a>).                

**Example**
@@@
```yaml
type: update
name: Base URL test
baseUrl: https://github.com/jelastic-jps/minio/blob/master

onInstall:
  log: Base URL test
  
onAfterRestartNode [cp]:
  script: build-cluster.js
  
success: README.md
```
``` json
{
    "type" : "update",
    "name" : "Base URL test",
    "baseUrl" : "https://github.com/jelastic-jps/minio/blob/master",
    "onInstall" : {
        "log" : "Base URL test"
    },
    "onAfterRestartNode [cp]" : {
        "script" : "build-cluster.js"
    },
    "success" : "README.md"
}
```
@@!

In case of the manifest installation via URL by means of the Jelastic **Import** functionality, the `baseUrl` placeholder will be defined if the specified path is set as in the example below:      
  
```
${baseUrl}/manifest.jps
```
where:                

- ${baseUrl}={protocol}//{domain}/{path}
- <b>*{protocol}*</b> - *http* or *https* protocols              
- <b>*{domain}*</b> - domain name of the website, where the manifest is stored                     
- <b>*{path}*</b> - directory path                
- <b>*manifest.jps*</b> - name of the file jps package                   

There are the following Cloud Scripting rules applied while parsing file's relative path:   
                      
- `baseUrl` parameter is being defined                            
- verification that the linked file’s text doesn't contain whitespaces (including tabs and line breaks)                                     
- verification that the linked file’s text doesn't contain semicolons and round brackets                                  

If installation is being run from <a href="https://github.com/jelastic-jps" target="_blank">*GitHub*</a> and URL includes <b>*‘/blob/’*</b>, it will be replaced with <b>*‘/raw/’*</b>. In case the `baseUrl` parameter is defined without a slash at the end, it will be added automatically.              

There are a list of JPS blocks which can use resources from **related** links:

- `logo` - JPS application image is shown while jps installation
- `script` - <a href="/creating-manifest/actions/#script" target="_blank">action</a>, for executing javascript and java scripts
- `description` - information about JPS which is shown before install process
- `success` - message after successful application installation

Relative links in these blocks check a file availability by URL. If file by defined link is absent (404 response code) a simple text will be displayed in that blocks.

For example:

@@@
```yaml
type: update
name: Relative Path Detection
baseUrl: https://example.com/
success: text.txt
```
```json
{
  "type": "update",
  "name": "Relative Path Detection",
  "baseUrl": "https://example.com/",
  "success": "text.txt"
}
```
@@!

In the example above the text *text.txt* will be displayed in success email notification and in success window in Jelastic dashboard when JPS installation will be finished. If URL **https://example.com/text.txt** has any content then that content will be displayed.

The Cloud Scripting engine also supports a `${baseUrl}` placeholder. It can be used throughout the users’ customs scripts (within the <a href="/creating-manifest/actions/#cmd" target="_blank">*cmd*</a> and <a href="/creating-manifest/actions/#script" target="_blank">*script*</a> actions).                 

For example:
@@@
```yaml
type: update
name: Test Base URL
baseUrl: http://example.com/

onInstall:
  cmd [cp]: curl -fSs '${baseUrl}script.sh'    
```
``` json
{
  "type" : "update",
  "name" : "Test Base URL",
  "baseUrl" : "http://example.com/",
  "onInstall" : {
    "cmd [cp]" : {
      "curl -fSs '${baseUrl}script.sh'"
    }
  }
}
```
@@!
                        
