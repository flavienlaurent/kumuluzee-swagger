# KumuluzEE Swagger
[![Build Status](https://img.shields.io/travis/kumuluz/kumuluzee-swagger/master.svg?style=flat)](https://travis-ci.org/kumuluz/kumuluzee-swagger)

> KumuluzEE Swagger extension provides powerful tools to incorporate and visualize the Swagger (OpenAPI 2.0) specification to your microservice.

KumuluzEE Swagger (OpenAPI 2.0) extension provides support for documenting APIs using Swagger/OpenAPI v2 compliant annotations. Extension automatically hooks-up servlet that 
exposes API specification on endpoint ```/api-specs/<jax-rs application-base-path>/swagger.[json|yaml]```. Extension also provides SwaggerUI which is added to your project
to visualize API documentation and allow API consumers to interact with API endpoints.
 
More details: [Swagger Specification](https://github.com/OAI/OpenAPI-Specification/blob/3.0.0/versions/2.0.md).

## Usage

You can enable the KumuluzEE Swagger support by adding the following dependency:
```xml
<dependency>
    <groupId>com.kumuluz.ee.swagger</groupId>
    <artifactId>kumuluzee-swagger</artifactId>
    <version>${kumuluzee-swagger.version}</version>
</dependency>
```

## Swagger configuration

When kumuluzee-swagger dependency is included in the project, you can start documenting your REST API using [Swagger-Core Annotations](https://github.com/swagger-api/swagger-core/wiki/Annotations-1.5.X).

### Documenting application class
```java
@SwaggerDefinition(info = @Info(title = "CustomersAPI", version = "v1.0.0"), host = "localhost:8080")
@ApplicationPath("v1")
public class CustomerApplication extends Application { ... }
```

### Documenting resource class and operations
```java
@Path("customer")
@Api
@Produces(MediaType.APPLICATION_JSON)
public class CustomerResource {

    @GET
    @ApiOperation(value = "Get customers list", tags = {"customers"}, notes = "Returns a list of customers.")
    @ApiResponses(value = {@ApiResponse(message = "List of customers", code = 200, response = Customer.class)})
    public Response getCustomers() {

        List<Customer> customers = new ArrayList<>();
        Customer c = new Customer("1", "John", "Doe");

        customers.add(c);

        return Response.status(Response.Status.OK).entity(customers).build();
    }
}
```

## Accessing API specification

Build and run project using:

```bash
mvn clean package
java -jar target/${project.build.finalName}.jar
```

After startup API specification will be available at:

**http://<-hostname-:<-port->/api-specs/<-application-base-path->/swagger.[json,yaml]**

Example:

http://localhost:8080/api-specs/v1/swagger.json

Serving Swagger specification can be disabled by setting property **kumuluzee.swagger.spec.enabled** to false. By default serving API spec is enabled.


## Adding Swagger-UI

To serve API specification in visual form and to allow API consumers to interact with API resources you can add Swagger-UI by setting 
 **kumuluzee.swagger.ui.enabled** to ***true***, by default UI is not included.

```yaml
kumuluzee:
  swagger:
      ui:
        enabled: true
```

After startup Swagger-UI is available at: http://localhost:8080/api-specs/ui (for all APIs).

By default Swagger-UI will not be added to application, you have to explicitly set the above property to true.


## Changelog

Recent changes can be viewed on Github on the [Releases Page](https://github.com/kumuluz/kumuluzee-swagger/releases)


## Contribute

See the [contributing docs](https://github.com/kumuluz/kumuluzee-swagger/blob/master/CONTRIBUTING.md)

When submitting an issue, please follow the 
[guidelines](https://github.com/kumuluz/kumuluzee-swagger/blob/master/CONTRIBUTING.md#bugs).

When submitting a bugfix, write a test that exposes the bug and fails before applying your fix. Submit the test 
alongside the fix.

When submitting a new feature, add tests that cover the feature.


## License

MIT
