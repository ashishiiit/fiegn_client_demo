1. configure openfiegn depedency.

  <dependency>
	 <groupId>org.springframework.cloud</groupId>
	 <artifactId>spring-cloud-starter-openfeign</artifactId>
  </dependency>
  
2. create service which you want to consume using fiegn clien.

		@RestController
		@RequestMapping("/coupan")
		public class CoupanController {
			@RequestMapping("/getCoupan")
			public double getCoupan() {
				return 10.5;
			}
		}

here i have created getCoupan() which i wan to consume in product-service.
  
2. create one interface with any name where you want to consume service and give @FeignClient and pass those service  name which you want to call.

	@FeignClient("COUPAN-SERVICE")
	public interface FiegnClient {
		
		@RequestMapping("/coupan/getCoupan")   //pass uri of the method which you want to comsune.
		public double getCoupan();
	
	} 
	
	Ex. i wan to call "COUPAN-SERVICE" in product so i have created one interface with FiegnClient in product-service project.
	here i am want to call "COUPAN-SERVICE" in product service so i here i pass COUPAN-SERVICE inside @FeignClient annotation.
	and create one abstract method whiwhc you want to call from product service(same as which method you want to call of coupan-service).

3. Enable FiegnCLient just like Eanble Eureka client or Eureka server.

	@SpringBootApplication
	@EnableEurekaClient
	//enable fiegnclient using @EnableFeignClients
	@EnableFeignClients
	public class ProductServiceApplication {
	
		public static void main(String[] args) {
			SpringApplication.run(ProductServiceApplication.class, args);
		}
	
	}

4. in controller inject inetrface (FiegnClient) and call method which you want to consume.

	@RestController
	@RequestMapping("/product")
	public class ProductController {
	
		@Autowired
		private FiegnClient fiegnClient;
		
		@RequestMapping("/getProduct")
		public Product saveProduct(@RequestBody Product p) {
		
		    //consume coupan-service method.
			double discount = fiegnClient.getCoupan();
			
			p.setProductName("apple phone");
			p.setPrice(p.getPrice() - discount);
			return p;	
		}
	}
	
 
