# CustomOAuth
CustomOAuth - OAuth 2.0 Authorization Server
This application is a custom-built OAuth 2.0 authorization server using Spring Security, providing endpoints for role-based access control, JWT token generation, and user authorization. It uses RSA public and private keys for secure token handling.

Features
JWT Authentication: JWT token-based authentication using RSA keys.
Role-based Access Control: Admin and User roles with access restrictions.
Custom Authentication and Authorization: Handles user registration and login, with role-based access for specific URLs.
Prerequisites
Java 17 or higher
MySQL 8 or higher
Spring Boot 3.x
Postman or any similar API testing tool
Setup Instructions
Clone the Repository: Clone the repository to your local machine:

bash
Copy code
git clone https://github.com/your-repository-url.git
cd customouth
Set up MySQL Database: Ensure you have a MySQL database named user_data running on localhost:3306.

Create the database:
sql
Copy code
CREATE DATABASE user_data;
Provide your MySQL database credentials in the application.properties file.
Application Configuration: Edit the application.properties file to reflect your own MySQL credentials:

properties
Copy code
spring.datasource.url=jdbc:mysql://localhost:3306/user_data?useSSL=false&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=Muke@2024
Run the Application: Run the application with the following command:

bash
Copy code
./mvnw spring-boot:run
Endpoints
1. Admin Access
URL: /admin/
Access: Only users with ROLE_ADMIN can access this endpoint.
Method: GET
Response:
json
Copy code
"Admin level access"
Postman Configuration:

Method: GET
URL: http://localhost:8000/admin/
Authorization:
Type: Bearer Token
Token: <valid-jwt-token-with-role-admin>
Response Example:
json
Copy code
"Admin level access"

================================

2. User Access
URL: /user/
Access: Only users with ROLE_ADMIN or ROLE_USER can access this endpoint.
Method: GET
Response:
json
Copy code
"User access level"
Postman Configuration:

Method: GET
URL: http://localhost:8000/user/
Authorization:
Type: Bearer Token
Token: <valid-jwt-token-with-role-user-or-admin>
Response Example:
json
Copy code
"User access level"

=================================

3. JWT Token Generation (Login)
URL: /auth/login
Method: POST
Request Body (Raw JSON):
json
Copy code
{
  "username": "user1",
  "password": "password123"
}
Response:
json
Copy code
{
  "user": {
    "userId": 1,
    "username": "user1",
    "roles": ["ROLE_USER"]
  },
  "jwt": "your-generated-jwt-token"
}
Postman Configuration:

Method: POST
URL: http://localhost:8000/auth/login
Body:
Type: raw (choose JSON format)
Content:
json
Copy code
{
  "username": "user1",
  "password": "password123"
}
Response Example:
json
Copy code
{
  "user": {
    "userId": 1,
    "username": "user1",
    "roles": ["ROLE_USER"]
  },
  "jwt": "your-generated-jwt-token"
}

=================================

4. Protected Resources (via JWT)
All other endpoints are protected by JWT authentication, and users must provide a valid JWT token in the Authorization header.
Postman Configuration:

Authorization:
Type: Bearer Token
Token: <your-jwt-token>
Headers:
Authorization: Bearer <your-jwt-token>
Example Postman Collection
You can import this Postman collection for easy access to the above endpoints:

Open Postman.
Click Import.
Choose Link and paste the following URL:
arduino
Copy code
https://www.getpostman.com/collections/your-postman-collection-url
Alternatively, you can manually configure each request as mentioned above.
Security Configuration
1. Role-Based Access Control (RBAC)
Admin Role: Only users with ROLE_ADMIN can access the /admin/ endpoint.
User Role: Users with ROLE_USER and ROLE_ADMIN can access /user/.
2. Session Management
The application uses stateless session management, relying solely on JWT tokens for authentication. This means no session is maintained on the server side.
3. RSA Keys for JWT Encryption
The application uses RSA keys (public and private) to sign and verify JWT tokens.
You can find the generated keys in the RSAKeyProperties class.
