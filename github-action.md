# Github Action

## Cache and run in orther job

  ```yml
  name: Test
  run-name: Test
  on:
    push:
      branches: [dev, test/github-action]
  jobs:
    build:
      runs-on: ubuntu-latest
      steps:
        - run: docker pull hello-world
        - run: docker save -o hello-world.tar hello-world
        - run: ls
        - uses: actions/cache@v3
          with:
            path: |
              hello-world.tar
            key: hello-world

    run:
      needs: build
      runs-on: ubuntu-latest
      steps:
        - uses: actions/cache@v3
          with:
            path: |
              hello-world.tar
            key: hello-world
        - run: ls
        - run: docker load -i hello-world.tar
        - run: docker run hello-world
  ```

## Example

  ```yml
  name: IT4U build and deploy to fly.io
  run-name: 🚀 Build and deploy to fly.io by ${{ github.actor }}
  on:
    push:
      branches: [dev, test/github-action]
  jobs:
    build:
      runs-on: ubuntu-latest
      steps:
        - name: Checkout
          uses: actions/checkout@v3

        - name: Setup JDK
          uses: actions/setup-java@v3
          with:
            distribution: 'temurin'
            java-version: 17
            cache: 'gradle'

        # - name: Login to GitHub Container Registry
        #   uses: docker/login-action@v2
        #   with:
        #     registry: ghcr.io
        #     username: ${{ github.repository_owner }}
        #     password: ${{ secrets.GHCR_TOKEN }}

        # - name: Login to Docker Hub
        #   uses: docker/login-action@v2
        #   with:
        #     username: ${{ secrets.DOCKERHUB_USER }}
        #     password: ${{ secrets.DOCKERHUB_TOKEN }}

        - name: Build with Gradle
          run: ./gradlew bootBuildImage

        - name: Save Docker Image
          run: docker save -o it4u-server.tar tpscvn/app:it4u-server-latest

        - name: Cache Docker Image
          uses: actions/cache@v3
          with:
            path: it4u-server.tar
            key: ${{ runner.os }}-docker-${{ hashFiles('it4u-server.tar') }}

        # - name: Push Docker image to GitHub Container Registry and Docker Hub
        #   run: |
        #     docker push ghcr.io/toantnm/tps/it4u/server:latest &
        #     docker push tpscvn/app:it4u-server-latest &


    push-to-docker-hub:
      needs: build
      runs-on: ubuntu-latest
      steps:
        - name: Cache Docker Image
          uses: actions/cache@v3
          with:
            path: it4u-server.tar
            key: ${{ runner.os }}-docker-${{ hashFiles('it4u-server.tar') }}

        - name: Load Docker Image
          run: docker load -i it4u-server.tar

        - name: Login to Docker Hub
          uses: docker/login-action@v2
          with:
            username: ${{ secrets.DOCKERHUB_USER }}
            password: ${{ secrets.DOCKERHUB_TOKEN }}

        - name: Push Docker image to GitHub Docker Hub
          run: docker push tpscvn/app:it4u-server-latest

    push-to-ghcr:
      needs: build
      runs-on: ubuntu-latest
      steps:
        - name: Cache Docker Image
          uses: actions/cache@v3
          with:
            path: it4u-server.tar
            key: ${{ runner.os }}-docker-${{ hashFiles('it4u-server.tar') }}

        - name: Load Docker Image
          run: |
            docker load -i it4u-server.tar
            docker tag tpscvn/app:it4u-server-latest ghcr.io/toantnm/tps/it4u/server:1.1.0

        - name: Login to GitHub Container Registry
          uses: docker/login-action@v2
          with:
            registry: ghcr.io
            username: ${{ github.repository_owner }}
            password: ${{ secrets.GHCR_TOKEN }}

        - name: Push Docker image to GitHub Container Registry
          run: docker push ghcr.io/toantnm/tps/it4u/server:1.1.0
  ```

## Build with `pack`

  ```yml
  build-with-pack:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Login to DockerHub Container Registry
        run: echo $DOCKER_HUB_TOKEN | docker login -u tpscvn --password-stdin
        env:
          DOCKER_HUB_TOKEN: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Install pack CLI via the official buildpack Action https://github.com/buildpacks/github-actions#setup-pack-cli-action
        uses: buildpacks/github-actions/setup-pack@v5.0.0

      - name: Build app with pack CLI using Buildpack Cache image (see https://buildpacks.io/docs/app-developer-guide/using-cache-image/) & publish to Docker Hub
        run: |
          pack build tpscvn/app:it4u-server-latest \
              --builder paketobuildpacks/builder:tiny \
              --path . \
              --env "BP_NATIVE_IMAGE=true" \
              --env "BP_JVM_VERSION=17" \
              --cache-image tpscvn/app-cache-image:latest \
              --publish

      - name: Setup flyctl
        uses: superfly/flyctl-actions/setup-flyctl@master

      - name: Deploy to fly.io
        run: flyctl deploy --local-only
        env:
          FLY_API_TOKEN: ${{ secrets.FLY_API_TOKEN }}
  ```