---

# Spring Boot E-Commerce CRUD Application

This project demonstrates a complete CRUD application using Spring Boot, Spring Data JPA and H2 Database.

The application allows users to:

- Add a product
- View all products
- View a product by Id
- Update a product
- Delete a product
- Search products
- Upload and retrieve product images

---

# Project Architecture

```
Client
   │
   ▼
Controller
   │
   ▼
Service
   │
   ▼
Repository (JpaRepository)
   │
   ▼
H2 Database
```

- Controller handles HTTP requests.
- Service contains business logic.
- Repository performs database operations.
- H2 stores product data.

---

# Product Entity

```java
@Entity
public class Product
```

The Product class represents a database table.

Important annotations:

- `@Entity` → Maps the class to a database table.
- `@Id` → Primary Key.
- `@GeneratedValue` → Generates Id automatically.
- `@Lob` → Stores large binary data (image).

Example:

```java
@Lob
private byte[] imageData;
```

Images are stored inside the database.

---

# File Upload using MultipartFile

Spring Boot provides the `MultipartFile` interface for uploading files.

Example:

```java
@PostMapping("/product")
```

```java
@RequestPart Product product,
@RequestPart MultipartFile imageFile
```

The uploaded image is converted into bytes before saving.

```java
imageFile.getBytes();
```

---

# Storing Image Information

Along with image bytes, additional information is also stored.

```java
imageName

imageType

imageData
```

This helps the application return the correct image format later.

---

# Returning Images

Images are returned using:

```java
ResponseEntity<byte[]>
```

Spring sends the image bytes along with the correct content type.

Example:

```java
contentType(MediaType.valueOf(product.getImageType()))
```

This allows browsers to display the image correctly.

---

# JpaRepository

The Repository extends:

```java
JpaRepository<Product, Integer>
```

Spring automatically provides CRUD methods such as:

- findAll()
- findById()
- save()
- deleteById()

No implementation class is required.

---

# Custom Query using JPQL

Spring Data JPA allows custom queries using `@Query`.

Example:

```java
@Query(...)
```

JPQL (Java Persistence Query Language) is similar to SQL, but it works with Entity classes instead of database tables.

In this project, the search query finds products by:

- Product Name
- Description
- Brand
- Category

The search is case-insensitive using:

```java
LOWER(...)
```

---

# Request Parameters

Search API:

```
GET /products/search?keyword=laptop
```

`@RequestParam` extracts values from query parameters.

Example:

```java
@RequestParam String keyword
```

---

# ResponseEntity

`ResponseEntity` is used to return:

- Response Body
- HTTP Status Code

Example:

```java
return new ResponseEntity<>(product, HttpStatus.OK);
```

Advantages:

- Better control over HTTP responses.
- Supports success and error responses.

---

# H2 Database

Configuration:

```properties
spring.datasource.url=jdbc:h2:mem:arun
```

Features:

- In-memory database.
- Lightweight.
- No installation required.
- Automatically created when the application starts.
- Ideal for development and testing.

---

# Overall Request Flow

```
Frontend (React)
        │
HTTP Request
        │
        ▼
ProductController
        │
        ▼
ProductService
        │
        ▼
ProductRepo
        │
        ▼
H2 Database
```

---

# New Spring Concepts Learned

- `@GeneratedValue`
- `@Lob`
- `MultipartFile`
- `@RequestPart`
- `ResponseEntity`
- `MediaType`
- `@RequestParam`
- `@Query`
- JPQL
- H2 Database Configuration

---

# Summary

- Spring Data JPA simplifies CRUD operations.
- `MultipartFile` is used to upload files.
- Images can be stored using `@Lob`.
- `ResponseEntity` provides complete HTTP responses.
- JPQL allows writing custom database queries.
- `@Query` is used for custom search operations.
- H2 is a lightweight in-memory database useful for development.
