# Java Gradle

1. Add properties before `<dependency>` tag

    ```groovy
    ext['spring-security.version']='5.8.2'
    dependencies {
      implementation 'org.springframework.boot:spring-boot-starter-actuator'
      implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
      implementation 'org.springframework.boot:spring-boot-starter-mail'
      implementation 'org.springframework.boot:spring-boot-starter-graphql'
      implementation 'org.springframework.boot:spring-boot-starter-security'
      implementation 'org.springframework.boot:spring-boot-starter-validation'
    }
    ```

1. Native Gradle way

    ```groovy
    dependencies {
        // import a BOM
        implementation platform('org.springframework.boot:spring-boot-dependencies:2.7.9')

        // define dependencies without versions
        implementation 'com.google.code.gson:gson'
        implementation 'dom4j:dom4j'
    }
    ```

- Recheck by this command:

  ```bash
  ./gradlew dependencyInsight --dependency=jackson-databind
  ```

**References:**

- <https://nexocode.com/blog/posts/spring-dependencies-in-gradle/>
