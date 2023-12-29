# Java-App
This is the Java App Demo
![image](https://github.com/quocbui0607/Java-App/assets/59446977/feb36d2c-deae-4b58-8d5a-74f1b2f75a99)

### Description
- This is the project I study from https://www.youtube.com/watch?v=mPPhcU7oWDU with:
  - product-service: store the data of product
  - order-service: handle the order workflow
  - notification-service: retrieve event from order-service then publish the notification
  - inventory-service: handle the logic of inventory for order workflow
  - api-gateway: authenticate user before forwarding request to other services
  - discovery-server: handle the finding server of services to forward requests (Eureka server)
### How to start project
- Install Postman
- Install Docker
- Once you have installed Docker, in the root of project - run command:
```
docker-compose up
```

- Check all the containers in Docker have started successfully

### How to test
- Navigate to ***localhost:8080*** to set up the client for authenticate by following steps:
  - Go to Administration Console with username/password: ***admin***
  - Create realm with name ***spring-boot-microservices-realm***
  - Then, create Client with name ***spring-cloud-client***
  - Turn on Client Authentication, Authorization and OAuth 2.0 Device Authorization Grant
  - After the client creation, go to credentials and copy the client Secret ![image](https://github.com/quocbui0607/Java-App/assets/59446977/866c9d45-4f06-457f-8d56-72ca7a76d76d)

- In Postman, add new request
```
curl --location 'http://localhost:8181/api/order' \
--header 'Content-Type: application/json' \
--data '{
    "orderLineItemsDtoList": [
        {
            "skuCode": "iphone_15",
            "price": 1300,
            "quantity": 1
        }
    ]
}'
```
- In the Authorization of request, change the type to ***OAuth 2.0***
- Add the configuration for token with:
  - Token name: Token
  - Grant type: Client Credientials
  - Access Token URL: http://keycloak:8080/realms/spring-boot-microservices-realm/protocol/openid-connect/token
  - Client ID: spring-cloud-client
  - Client Secret: Get from above in keycloak
![image](https://github.com/quocbui0607/Java-App/assets/59446977/c5d377cc-44bd-439f-a9fd-f36dbfc938bc)

- Navigate to bottom of Authorization tab, Click ***Get New AccessToken*** (This will fail)
- Go to ```C:\Windows\System32\drivers\etc``` add ***127.0.0.1 keycloak*** - Save file and test again
![image](https://github.com/quocbui0607/Java-App/assets/59446977/269b6110-f954-477c-829e-98f5af6ce897)

- Press proceed and use token
- The request will send 201 status code that your order have been placed
- Use docker logs to check the logs on each service
  
