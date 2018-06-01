# Marathon-Service
Marathon-Service


My deploy script for ELK, HDFS and Jenkins on Marathon.


#### elasticsearch as example
```
{
  "id": "/elk/elasticsearch",
  "cmd": "mv elasticsearch.yml elastic*/config && rm elasticsearch.tar.gz && cd ela* && ./bin/elasticsearch",
  "cpus": 1,
  "mem": 1500,
  "disk": 0,
  "instances": 1,
  "acceptedResourceRoles": [
    "*"
  ],
  "fetch": [
    {
      "uri": "http://192.168.2.104:6688/elasticsearch.yml",
      "extract": true,
      "executable": false,
      "cache": false
    },
    {
      "uri": "http://192.168.2.104:6688/elasticsearch.tar.gz",
      "extract": true,
      "executable": false,
      "cache": false
    }
  ],
  "portDefinitions": [
    {
      "port": 10001,
      "name": "default",
      "protocol": "tcp"
    }
  ],
  "user": "peterlee"
}

```

Put your binary at your nginx or other web server.    

In this example, I use elasticsearch.yml for config file. For binary, I pack binary to elasticsearch.tar.gz.
After Marathon container get these files, it will start to extract tar, then it start cmd.

```
mv elasticsearch.yml elastic*/config && rm elasticsearch.tar.gz && cd ela* && ./bin/elasticsearch
```

This script will move config file to elasticsearch config folder, then start command /bin/elasticsearch.



## Deploy

```
$ curl -X POST -d "@jenkins.json"  -H "Content-Type: application/json" http://localhost:8080/v2/apps
```

Result

```
{"id":"/jenkins","acceptedResourceRoles":["*"],"backoffFactor":1.15,"backoffSeconds":1,"cmd":"java -jar jenkins.war --httpPort=8888","cpus":1,"disk":0,"executor":"","fetch":[{"uri":"http://192.168.2.104:6688/jenkins.war","extract":false,"executable":false,"cache":false}],"instances":1,"labels":{},"maxLaunchDelaySeconds":3600,"mem":500,"gpus":0,"networks":[{"mode":"host"}],"portDefinitions":[{"port":10004,"name":"default","protocol":"tcp"}],"requirePorts":false,"upgradeStrategy":{"maximumOverCapacity":1,"minimumHealthCapacity":1},"user":"peterlee","version":"2018-06-01T03:14:35.490Z","killSelection":"YOUNGEST_FIRST","unreachableStrategy":{"inactiveAfterSeconds":0,"expungeAfterSeconds":0},"tasksStaged":0,"tasksRunning":0,"tasksHealthy":0,"tasksUnhealthy":0,"deployments":[{"id":"48727e00-39fe-48ba-a6b6-d83005f89063"}],"tasks":[]}%
```