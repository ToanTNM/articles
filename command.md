1. Maven

Run: `./mvnw spring-boot:run`

Clean build (to *.jar): `./mvnw clean package`

Clean build (to native):  `./mvnw clean package -Pnative`

Build to docker image: `./mvnw spring-boot:build-image`

1. Gradle

Run:  `./gradlew bootRun`

Clean build (to *.jar): `./gradlew clean build`

Build to docker image: `./gradlew bootBuildImage`

Change gradle version: `./gradlew wrapper --gradle-version=8.0.2`

1. `sdk` command

Install `sdk`

```bash
curl -s "https://get.sdkman.io" | bash
source "/home/tps/.sdkman/bin/sdkman-init.sh"
```

List version `sdk list java`

Install: `sdk install java 22.3.r19-grl`

`sdk install java 22.3.r17-nik`

Use specific version: `sdk use java 22.3.r17-nik`

1. `git` command

Push to git from existing project

```bash
git init
git add .
git commit -m 'Init'
git remote add origin git@github.com:ToanTNM/caligo.git
git branch -M main
git push -u origin main
```
