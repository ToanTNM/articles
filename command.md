# Often use command

1. `ubuntu` command

- Find folder by name
  
   ```sh
   # Find folder by name
   find /path/to/search -type d -name "your-folder-name"
   find /path/to/search -type d -iname "your-folder-name" # ignore case

   # Copy file by ssh
   scp source_file user@destination:/path/to/destination
   ```

1. `Maven` command

   Run: `./mvnw spring-boot:run`

   Clean build (to \*.jar): `./mvnw clean package`

   Clean build (to native): `./mvnw clean package -Pnative`

   Build to docker image: `./mvnw spring-boot:build-image`

1. `Gradle` command

   Run: `./gradlew bootRun`

   Clean build (to \*.jar): `./gradlew clean build`

   Build (to native): `./gradlew nativeCompile`

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

1. `docker` command

   ```sh
   docker run --rm -p 80:80 [imageName]
   
   docker logs [containerName or id] -f

   docker cp <containerId>:/file/path/within/container /host/path/target # copy file from container to host
   ```

1. `flyctl` / `fly` command

    ```sh
    fly postgres create

    flyctl postgres connect -a [app-name] # then we can run pg command

    flyctl proxy 5432 -a [app-name] # then we can connect via postgres://localhost:5432
    ```
