
/**
 * 
 * default ports for spring netflix tools
 * 
 */

//for Zuul
server.port = 8000
//for eureka
server.port=8761
//for zipkin
server.port=
//for hystric dashboard
server.port=
//for spring cloud config
server.port=8888
----------------------------------------------------------------------------------------------




/**
 * Zuul Api Gateway
 */

@SpringBootApplication
@EnableZuulProxy
public class ZullApiGatewayApplication {

	public static void main(String[] args) {
		SpringApplication.run(ZullApiGatewayApplication.class, args);
	}
}

// add bean of type sampler. (AlwaySampler) for sleuth

// extend abstract class of ZullFilter for manupliting each request coming to  Gateway

/**
 * application.properties
 *  */

zuul.prefix=/api


#disable zull's default dynamic service finding feature. as we 
#are providing our own routing.
zuul.ignored-services= '*'

#Routing table for routing to all microservices from UI
zuul.routes.a.path= /a/**
zuul.routes.a.service-id=servicea


#strip prefix means, whether to remove this 'a' from url. 
#we are setting it to false because, 'a' is a mapping in our controller.
#and we need this. so URL will be from 
#GET /localhost:8000/a/anything-here will convert to -->
# /localhost:8080/a/anything-here.

#if we set this property to true then, URL will be from
# GET /localhost:8000/a/anything-here will convert to -->
# /localhost:8080/anything-here.
# in short, it will remove 'a'
zuul.routes.a.strip-prefix=false

zuul.routes.b.path= /b/**
zuul.routes.b.service-id=serviceb
zuul.routes.b.strip-prefix=false


management.trace.http.enabled=true

management.endpoints.web.exposure.include=*

------------------------------------------------------------------------------------------------------


/**microservice config for eureka and zipkin **/

@SpringBootApplication
@EnableDiscoveryClient
public class MicroserviceApplication {

	public static void main(String[] args) {
		SpringApplication.run(MicroserviceApplication.class, args);
	}
}
eureka.client.service-url.default-zone=http:\\\\localhost:8761\\eureka\\

spring.cloud.refresh.extra-refreshable=false

#zipkin
spring.zipkin.service.name=currency-exchange-service
spring.zipkin.sender.type=web
spring.zipkin.baseUrl=http://localhost:8500
spring.sleuth.sampler.probability=1.0

----------------------------------------------------------------------------------------

/**
 * eureka server config
 */


eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false


logging.level.com.netflix.eureka=OFF
logging.level.com.netflix.discovery=OFF

-------------------------------------------------------------------------------------------------------------

