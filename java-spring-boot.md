# Spring boot Notes

## 1. Một số khái niệm và Annotation cơ bản

>```@SpringBootApplication```

  Nếu trong Java truyền thống, khi chạy cả một project, chúng ta sẽ phải định nghĩa một hàm ```main()``` và để nó khởi chạy đầu tiên. Điều này không có gì thay đổi!

  Có khác là chúng ta sẽ phải chỉ cho Spring Boot biết nơi nó khởi chạy lần đầu, để nó cài đặt mọi thứ.

  Cách thực hiện là thêm annotation ```@SpringBootApplication``` trên class chính và gọi ```SpringApplication.run(App.class, args);``` để chạy project.

  ```SpringApplication.run(App.class, args)``` là câu lệnh để tạo ra **container**. Sau đó nó scan toàn bộ các **dependency** trong project và đưa vào đó.

  Spring đặt tên cho **container** là ```ApplicationContext```, các **dependency** là ```Bean```

>```@Component```

  ```@Component``` là một **Annotation** đánh dấu trên các ```Class``` để giúp Spring biết nó là một ```Bean```.

  >```@Autowired```

  Dùng cho *attribute* của class, Spring boot sẽ tự động inject dependency tương ứng

  ```java
  //Outfit.java
  @Component
  public class Bikini implements Outfit {
      @Override
      public void wear() {
          System.out.println("Mặc bikini");
      }
  }

//Main class
  @SpringBootApplication
public class App {
    public static void main(String[] args) {
      // ApplicationContext chính là container, chứa toàn bộ các Bean
      ApplicationContext context = SpringApplication.run(App.class, args);

      // Khi chạy xong, lúc này context sẽ chứa các Bean có đánh
      // dấu @Component.

      // Lấy Bean ra bằng cách
      Outfit outfit = context.getBean(Outfit.class);

      // In ra để xem thử nó là gì
      System.out.println("Instance: " + outfit);
      // xài hàm wear()
      outfit.wear();
    }
}
  ```

  tương đương với

  ```java
  @SpringBootApplication
  public class App {
      public static void main(String[] args) {
        // ApplicationContext chính là container, chứa toàn bộ các Bean
        ApplicationContext context = SpringApplication.run(App.class, args);

        // Khi chạy xong, lúc này context sẽ chứa các Bean có đánh
        // dấu @Component.

        // Lấy Bean ra bằng cách
        @AutoWired
        Outfit outfit;

        // In ra để xem thử nó là gì
        System.out.println("Instance: " + outfit);
        // xài hàm wear()
        outfit.wear();
  ```

>```@Primary``` và ```@Qualifier```

  Các ```Bean``` được quản lý bên trong ```ApplicationContext``` đều là **singleton**. Nếu muốn mỗi lần sử dụng là một ***instance*** hoàn toàn mới. Thì hãy đánh dấu ```@Component``` đó bằng ```@Scope("prototype")```

  Khi Spring Boot có chứa 2 ```Bean``` cùng loại trong Context Spring sẽ không biết sử dụng ```Bean``` nào để inject vào đối tượng và báo lỗi khi chạy. Trường hợp này, dùng annotation ```@Primary``` để thông báo cho **Spring** biết ưu tiên inject class nào hoặc dùng ```@Qualifier``` để chỉ định tên Bean khi inject:

  ```java
  @Component("bikini")
  public class Bikini implements Outfit {
      @Override
      public void wear() {
          System.out.println("Mặc bikini");
      }
  }

  @Component("naked")
  public class Naked implements Outfit {
      @Override
      public void wear() {
          System.out.println("Đang không mặc gì");
      }
  }

  @Component
  public class Girl {

      Outfit outfit;

      // Đánh dấu để Spring inject một đối tượng Outfit vào đây
      public Girl(@Qualifier("naked") Outfit outfit) {
          // Spring sẽ inject outfit thông qua Constructor đầu tiên
          // Ngoài ra, nó sẽ tìm Bean có @Qualifier("naked") trong context để ịnject
          this.outfit = outfit;
      }

      // GET
      // SET
  }
  ```

>```@PostConstruct```, ```@PreDestroy```

  ```@PostConstruct``` được đánh dấu trên một method duy nhất trong ứng dụng. ```IoC Container``` hoặc ```ApplicationContext``` sẽ gọi hàm này sau khi một ```Bean``` được tạo ra và quản lý

  ```@PreDestroy``` được đánh dấu trên một method duy nhất bên trong ứng dụng. ```IoC Container``` hoặc ```ApplicationContext``` sẽ gọi hàm này trước khi một Bean bị xóa hoặc không được quản lý nữa.

>```@Controller```, ```@Service```, ```@Repository```

  là các ```@Component```, dùng để đánh dấu các class theo chức năng trong mô hình MVC. Về bản chất các annotation ```@Controller```, ```@Service```, ```@Repository``` có thể thay thế cho nhau mà ứng dụng vẫn hoạt động bình thường

  ![Spring MVC structure](https://super-static-assets.s3.amazonaws.com/8a72ee8e-d4aa-4a06-985f-e92802c5bc44/images/f5ff73d5-5bd6-4d03-a0e1-da9418ec9a70.png?w=1500&f=webp)

  **Consumer Layer** hay **Controller**: là tầng giao tiếp với bên ngoài và handler các request từ bên ngoài tới hệ thống; được đánh dấu bằng ```@Controller```

  **Service Layer**: thực hiện các nghiệp vụ và xử lý logic; được đánh dấu bằng ```@Service```

  **Repository Layer**: giao tiếp với DB, thiết bị lưu trữ, xử lý query và trả về các kiểu dữ liệu mà tầng **Service** yêu cầu; được đánh dấu bằng ```@Repository```

>```@Configuration``` và ```@Bean```

  ```@Configuration``` là một annotation đánh dấu trên một ```Class``` cho phép **Spring Boot** biết được đây là nơi định nghĩa ra các Bean.

  ```@Bean``` là một Annotation được đánh dấu trên các method cho phép Spring Boot biết được đây là Bean và sẽ thực hiện đưa Bean này vào Context.   ```@Bean``` sẽ nằm trong các class có đánh dấu ```@Configuration```.

>```@Value``` được sử dụng trên attribute của class, Có nhiệm vụ lấy thông tin từ file ```.properties``` và gán vào biến.

> ```@RequestBody``` chuyển JSON thành Object tương ứng

  ```java
  @RestController
  @RequestMapping("/api/v1")
  public class RestApiController {

      List<Todo> todoList = new CopyOnWriteArrayList<>();

      @PostMapping("/todo")
      public ResponseEntity addTodo(@RequestBody Todo todo) {
          todoList.add(todo);
          // Trả về response với STATUS CODE = 200 (OK)
          // Body sẽ chứa thông tin về đối tượng todo vừa được tạo.
          return ResponseEntity.ok().body(todo);
      }

  }
  ```

>```@PathVariable``` lấy thông tin từ url

  ```java
  @RestController
  @RequestMapping("/api/v1")
  public class RestApiController {

      /*
      phần path URL bạn muốn lấy thông tin sẽ để trong ngoặc kép {}
      */
      @GetMapping("/todo/{todoId}")
      public Todo getTodo(@PathVariable(name = "todoId") Integer todoId){
          // @PathVariable lấy ra thông tin trong URL
          // dựa vào tên của thuộc tính đã định nghĩa trong ngoặc kép /todo/{todoId}
          return todoList.get(todoId);
      }
  }
  ```

>```@ResponseStatus``` định nghĩa Http Status trả về cho người dùng.

Nếu không muốn sử dụng ```ResponseEntity``` thì có thể dùng ```@ResponseStatus``` đánh dấu trên Object trả về.

>```@RestControllerAdvice``` & ```@ControllerAdvice``` + ```@ExceptionHandler```

  ```@RestControllerAdvice``` là một annotation gắn trên Class. Có tác dụng xen vào quá trình xử lý của các @RestController.

  ```@RestControllerAdvice``` thường được kết hợp với ```@ExceptionHandler``` để cắt ngang quá trình xử lý của Controller, và xử lý các ngoại lệ xảy ra

>```@ConfigurationProperties``` dùng để đánh dấu class bên dưới nó là properties, các thuộc tính sẽ được tự động nạp vào khi Spring khởi tạo

  ```java
  @Data // Lombok, xem chi tiết tại bài viết
  @Component // Là 1 spring bean
  // @PropertySource("classpath:loda.yml") // Đánh dấu để lấy config từ trong file loda.yml
  @ConfigurationProperties(prefix = "loda") // Chỉ lấy các config có tiền tố là "loda"
  public class LodaAppProperties {
    private String email;
    private String googleAnalyticsId;

    private List<String> authors;

    private Map<String, String> exampleMap;
  }

  @SpringBootApplication
  @EnableConfigurationProperties
  public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }

    @Autowired LodaAppProperties lodaAppProperties;

    @Override
    public void run(String... args) throws Exception {
      System.out.println("Global variable:");
      System.out.println("\t Email: " + lodaAppProperties.getEmail());
      System.out.println("\t GA ID: " + lodaAppProperties.getGoogleAnalyticsId());
      System.out.println("\t Authors: " + lodaAppProperties.getAuthors());
      System.out.println("\t Example Map: " + lodaAppProperties.getExampleMap());
    }
  }
  ```

  ```yml
  loda:
    email: loda.namnh@gmail.com
    googleAnalyticsId: U-xxxxx
    authors:
      - loda
      - atom
    exampleMap:
      key1: hello
      key2: world
  ```

><https://loda.me/courses/spring-boot>
