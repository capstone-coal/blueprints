# blueprints
Blueprints which ensure that [COAL-SDS](https://github.com/capstone-coal/coal-sds) can run on any clouds in any containers, anywhere... powered by [Apache Brooklyn](https://brooklyn.apache.org/).

## Installation
Install Apache Brooklyn (note this will not work if you are running JDK11... you may wish to use the Docker build for Brooklyn)
```
// first change into directory you wish to install Brooklyn
$ curl -SL --output apache-brooklyn-0.12.0-bin.tar.gz "https://www.apache.org/dyn/closer.lua?action=download&filename=brooklyn/apache-brooklyn-0.12.0/apache-brooklyn-0.12.0-bin.tar.gz"
$ tar xvf apache-brooklyn-0.12.0-bin.tar.gz
$ cd apache-brooklyn-0.12.0-bin
$ ./bin/start
```

### Docker build for Brooklyn
```
$ git clone http://github.com/apache/brooklyn/
$ cd brooklyn
$ git submodule init
$ git submodule update --remote --merge --recursive
$ docker build -t brooklyn .
$ docker run -i --rm --name brooklyn -u $(id -u):$(id -g) \
      --mount type=bind,source="${HOME}/.m2/settings.xml",target=/var/maven/.m2/settings.xml,readonly \
      -v /var/run/docker.sock:/var/run/docker.sock \
      -v ${PWD}:/usr/build -w /usr/build \
      brooklyn mvn clean install -Duser.home=/var/maven -Duser.name=$(id -un)
$ pushd brooklyn-dist/karaf/apache-brooklyn/target/assembly/
$ ./bin/start
```

## License

All of the code is licensed under the [Apache Licence v2.0](https://www.apache.org/licenses/LICENSE-2.0), a copy of which ships with this repository.
