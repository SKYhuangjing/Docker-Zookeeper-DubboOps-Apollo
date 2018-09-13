
![Pinpoint](https://github.com/naver/pinpoint-docker/blob/master/docs/logo.png)

# Pinpoint-Docker for Pinpoint

Official git repository of Dockerized components of the [Pinpoint Application Monitoring](http://naver.github.io/pinpoint/).  
Installing Pinpoint with these docker files will take approximately 10min. to check out the features of pinpoint.

## What is Pinpoint

[Pinpoint](https://github.com/naver/pinpoint), is the world's leading open-source application monitoring solution - trusted by millions of users around the globe.
It supports and helps you understand your application in a glance and allow you to build world-class, high-quality software.

## Supported Tags

 - 1.7.3
 - 1.7.2

## How to install Pinpoint?

You can easily bring up an entire Dockerized Pinpoint(latest release) environment by using [Docker Compose](https://docs.docker.com/compose/) with any of the provided `docker-compose.yml` files  as below.  
With `docker-compose.yml` under *Pinpoint-Docker* folder brings up all the environment attached with Pinpoint-QuickStart(sample app).
To monitor your agent see [configuration part](#configurations) for further details.  

```
git clone https://github.com/naver/pinpoint-docker.git
cd Pinpoint-Docker
docker-compose pull && docker-compose up -d
```
If you'd like to bring up the previous release. Try with docker-compose file from other tags. 
You can also just build the image with `docker-compose up -d` command without pulling the image. But you can reduce the time to 1/3 by just downloading them.

This will install and run all services required to run all features in Pinpoint in docker containers joined with same network.
 - Pinpoint-Web Server
 - Pinpoint-Collector
 - Pinpoint-Agent(ready to be used)
 - Pinpoint-Flink
 - Pinpoint-Zookeeper
 - Pinpoint-Hbase
 - Pinpoint-QuickStart(a sample application)  
This may take several minutes to download all necessary images.

You can modify `QuickStart` application part with your application to start monitoring.  
(see [`Monitoring YOUR Application`](#monitoring-your-application) part for further details).

### Flink configuration

After all containers are started and ready to go. There is one more thing to do to use all existing features in Pinpoint.
It's not mandatory, but to use all the features and since it's a simple task, let's take care of it.

Register a `job` on to pinpoint-flink server.  
You can build the `job` from the [open-source of Pinpoint](https://github.com/naver/pinpoint), additional guide is [here](https://github.com/naver/pinpoint/blob/master/doc/application-inspector.md#application-inspector).   
or  
you can simply upload pre-built jar file under pinpoint-flink/build/pinpoint-flink-job-{pinpoint-version}.jar (beware of the version, it should matched with *PINPOINT_VERSION* in .env file)  
If anyone have solution to put the job file into flink image without doing manually, please let us know.

Pinpoint-Flink server is running on [port 8081](http://localhost:8081/#/submit). From `submit new job` menu
Submit the jar file with *com.navercorp.pinpoint.flink.StatStreamingVer2Job* in entry class as below image.

![Pinpoint](https://github.com/naver/pinpoint-docker/blob/master/docs/Pinpoint-Flink%20upload.png)
 
Now you are ready to monitor the sample application(Pinpoint-QuickStart) provided.
If you can't find any connected application from Pinpoint-Web's first page([port 8079](http://localhost:8079) as default), don't panic and wait for a while.
It will take some time for Pinpoint to retrieve the application's information when running for the first time.

## Monitoring YOUR Application

Pinpoint-Agent only prepares required libraries for triggering Pinpoint-Agent.
Running and configuring agents is manual action done by the user, but don't worry it's very simple.

If you are not familiar with Pinpoint concept, please read: [Overview](http://naver.github.io/pinpoint/overview.html#architecture),
[Agents Installation](http://naver.github.io/pinpoint/installation.html#5-pinpoint-agent)

**You will need to attach *Pinpoint-Agent* to your application.**

Running Pinpoint-Agent docker-compose separately, Examples are [here](https://github.com/naver/pinpoint-docker/tree/master/pinpoint-agent-attach-example).  
Otherwise, you can check how [Pinpoint-Quickstart](https://github.com/naver/pinpoint-docker/blob/master/docker-compose.yml) is attached to Pinpoint-Agent with docker-compose.

We'll try to create more examples along the way.
If anyone who can share their dockerfile, it's always welcome.

## Distributed System

Until now, every components are in one docker, single-node approach, which is excellent for test and development.
It provides an easy way to prototype new ideas and use cases, as well as try out new functionality and the latest Pinpoint releases.
It’s not intended nor supported for production use.

You can use `docker-compose` and `.env` files under each folder to install the modules separately into several servers.
If containers are separated, ip configurations in `.env` must be changed within. 

For example, if you want your application running from a docker and rest of Pinpoint in another.
You can remove *pinpoint-agent* and *pinpoint-quickstart* from docker-compose.yml and run to establish all necessary component of pinpoint.
And create another docker-compose.yml just like one under pinpoint-quickstart folder to run your application.
Finally, since agent needs to acknowledge the collector ip. collector ip needs to be changed in .env.

## Configurations

Configuration relies on supplying `docker-compose` with environment variables defined in `.env` file. So it's recommended to change variables only from `.env` file.
With `docker-compose` in this repository. You can create stand-alone containers that are needed to run most of the features in Pinpoint.

**Ports** can be also configured in .env file.
(Default ports are Pinpoint-Web:8079, Quickstart:8000 and Flink:8081 as configured in .env file)

Pinpoint-Zookeeper is just an example of using zookeeper image. You can modify docker-compose files to suit your needs.

For more specific details on what the values represents in *.env* file. Please check [Pinpoint Github Repository](https://github.com/naver/pinpoint) or
[Pinpoint Web properties](https://github.com/naver/pinpoint/blob/master/web/src/main/resources/pinpoint-web.properties), [Pinpoint Collector properties](https://github.com/naver/pinpoint/blob/master/collector/src/main/resources/pinpoint-collector.properties), [Pinpoint Agent configuration](https://github.com/naver/pinpoint/blob/master/agent/src/main/resources-release/pinpoint.config).  
Please note that only essential configuration options are adopted to pinpoint-docker(docker-compose). 
 
## logs 
 
You can check logs produced by these services
 ```
 docker logs <containerId>
 ```
 
You can also easily change the log level from *.env* file. 
 
## Any Issues or Suggestions?

Feel free to share any problems and suggestions via [Pinpoint GitHub Issue page](https://github.com/naver/pinpoint/issues).
Contributions on the pinpoint-docker image is also always welcome.

## License
Pinpoint is licensed under the Apache License, Version 2.0.
See [LICENSE](https://github.com/naver/pinpoint/blob/master/LICENSE) for full license text.

```
Copyright 2018 NAVER Corp.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

