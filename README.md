# docker-scala-sbt

Enables build of Scala apps.

This has:
- sbt
- activator
- OpenJDK
- sonar-scanner

### Usage

This image is to be used to build your scala apps. Below in an example of how you would do this in drone with caching enabled.

Contents of .drone.yml:
```
pipeline:
  drone_s3_cache_pull:
     image: quay.io/ukhomeofficedigital/drone-s3cache:v0.1.0
     drone_s3_cache_mode: "pull"

  build:
    commands:
       - "/root/entrypoint.sh 'sbt clean update test assembly'"
    image: quay.io/ukhomeofficedigital/scala-sbt:v0.2.0
    when:
      event:
        - push
        - pull_request

  drone_s3_cache_push:
    image: quay.io/ukhomeofficedigital/drone-s3cache:v0.1.0
    drone_s3_cache_folders:
      - .ivy2
    drone_s3_cache_mode: "push"
```
