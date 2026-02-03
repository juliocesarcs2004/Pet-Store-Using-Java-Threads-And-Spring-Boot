# 🐾 Pet-Store-Using-Java-Threads-And-Spring-Boot

A modern e-commerce application for a pet products store, developed with **Java**, **Spring Boot**, and **MySQL**, implementing advanced concepts of security, asynchronous threads, and order processing.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Technologies Used](#technologies-used)
- [Project Architecture](#project-architecture)
- [Directory Structure](#directory-structure)
- [Data Models](#data-models)
- [API Endpoints](#api-endpoints)
- [Authentication and Security](#authentication-and-security)
- [Main Features](#main-features)
- [Configuration and Installation](#configuration-and-installation)
- [Running the Application](#running-the-application)
- [Workflow](#workflow)

---

## 🎯 Overview

**Adopet Store** is an e-commerce platform developed in Spring Boot 3.2 that manages:
- ✅ Pet products with categories
- ✅ Real-time inventory system
- ✅ Order processing with validation
- ✅ Authentication and role-based authorization (RBAC)
- ✅ Asynchronous email sending using threads
- ✅ Inventory and billing reports
- ✅ Security with JWT and BCrypt

---

## 🛠️ Technologies Used

### Backend
| Technology | Version | Purpose |
|-----------|--------|---------|
| **Java** | 21 | Primary language |
| **Spring Boot** | 3.2.0 | Web framework |
| **Spring Data JPA** | 3.2.0 | ORM and data access |
| **Spring Security** | 3.2.0 | Authentication and authorization |
| **Spring Mail** | 3.2.0 | Email sending |
| **MySQL** | - | Relational database |
| **Flyway** | 9.22.2 | Database versioning |
| **Java JWT** | 4.4.0 | JWT token generation and validation |
| **Maven** | - | Dependency manager |

### Main Dependencies
- `spring-boot-starter-web` - REST APIs
- `spring-boot-starter-data-jpa` - Data persistence
- `spring-boot-starter-security` - Security
- `spring-boot-starter-validation` - Validations
- `spring-boot-starter-mail` - Email
- `mysql-connector-j` - MySQL driver
- `flyway-core` and `flyway-mysql` - Database migrations
- `com.auth0:java-jwt:4.4.0` - JWT

---

## 🏗️ Project Architecture

The application follows a **layered MVC architecture with service layer**, organized as follows:

```
┌─────────────────────────────────────────┐
│         Controllers (REST API)           │
│  (Receive HTTP requests)                 │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│      Services (Business Logic)           │
│  (Processing and validations)            │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│  Repositories (Data Access - JPA)       │
│  (Database operations)                   │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│       MySQL Database                     │
│  (Permanent persistence)                 │
└─────────────────────────────────────────┘
```

### Security Flow

```
HTTP Request
      │
      ▼
┌─────────────────────────┐
│  SecurityFilter         │
│  (Validates JWT Token)  │
└────────┬────────────────┘
         │
      Valid?
      / \
    Yes  No
    /     \
   ✓       ✗ (Error 403)
   │
   ▼
Controller
   │
   ▼
Service (Business Logic)
   │
   ▼
Repository/Database
```

---

## 📁 Directory Structure

```
adopet-store/
│
├── src/
│   ├── main/
│   │   ├── java/br/com/alura/adopetstore/
│   │   │   ├── AdopetStoreApplication.java          # Main class
│   │   │   │
│   │   │   ├── controller/                           # REST Controllers
│   │   │   │   ├── ProdutoController.java
│   │   │   │   ├── PedidoController.java
│   │   │   │   ├── EstoqueController.java
│   │   │   │   ├── LoginController.java
│   │   │   │   └── RelatorioController.java
│   │   │   │
│   │   │   ├── service/                              # Business Services
│   │   │   │   ├── ProdutoService.java
│   │   │   │   ├── PedidoService.java
│   │   │   │   ├── EstoqueService.java
│   │   │   │   ├── UsuarioService.java
│   │   │   │   └── RelatorioService.java
│   │   │   │
│   │   │   ├── repository/                           # Data Access
│   │   │   │   ├── ProdutoRepository.java
│   │   │   │   ├── PedidoRepository.java
│   │   │   │   ├── EstoqueRepository.java
│   │   │   │   └── UsuarioRepository.java
│   │   │   │
│   │   │   ├── model/                                # JPA Entities
│   │   │   │   ├── Produto.java
│   │   │   │   ├── Pedido.java
│   │   │   │   ├── ItemPedido.java
│   │   │   │   ├── Estoque.java
│   │   │   │   ├── Usuario.java
│   │   │   │   ├── Perfil.java
│   │   │   │   └── Categoria.java (enum)
│   │   │   │
│   │   │   ├── dto/                                  # Data Transfer Objects
│   │   │   │   ├── CadastroProdutoDTO.java
│   │   │   │   ├── CadastroPedidoDTO.java
│   │   │   │   ├── CadastroUsuarioDTO.java
│   │   │   │   ├── LoginDTO.java
│   │   │   │   ├── ProdutoDTO.java
│   │   │   │   ├── PedidoDTO.java
│   │   │   │   ├── EstoqueDTO.java
│   │   │   │   ├── ItemPedidoDTO.java
│   │   │   │   ├── RelatorioEstoque.java
│   │   │   │   ├── RelatorioFaturamento.java
│   │   │   │   └── EstatisticasVenda.java
│   │   │   │
│   │   │   ├── security/                             # Security
│   │   │   │   ├── SecurityConfigurations.java       # Spring Security Config
│   │   │   │   ├── SecurityFilter.java               # JWT Filter
│   │   │   │   └── TokenService.java                 # JWT Generation/Validation
│   │   │   │
│   │   │   ├── email/                                # Email Sending
│   │   │   │   ├── EnviadorEmail.java                # Base service with @Async
│   │   │   │   └── EmailPedidoRealizado.java         # Order confirmation email
│   │   │   │
│   │   │   └── exception/                            # Error Handling
│   │   │       ├── ValidacaoException.java
│   │   │       └── TratadorDeErros.java
│   │   │
│   │   └── resources/
│   │       ├── application.properties                # App configurations
│   │       └── db/migration/
│   │           ├── V001__create-table-usuarios.sql   # Users and Roles
│   │           ├── V002__create-table-produtos.sql   # Products
│   │           ├── V003__create-table-estoques.sql   # Inventory
│   │           └── V004__create-table-pedidos.sql    # Orders and Items
│   │
│   └── test/
│       └── java/...                                   # Unit tests
│
├── pom.xml                                             # Maven Configuration
├── mvnw / mvnw.cmd                                     # Maven Wrapper
└── README.md                                           # This file
```

---

## 📊 Data Models

### 1. **Usuario** (System Users)
```java
@Entity
public class Usuario implements UserDetails {
    @Id @GeneratedValue Long id;
    String nome;
    String email;         // Unique
    String senha;         // Encrypted with BCrypt
    @ManyToMany List<Perfil> perfis;  // ROLE_ADMIN or ROLE_COMPRADOR
}
```

**Roles (Profiles):**
- `ROLE_ADMIN` - Full access, manages products and reports
- `ROLE_COMPRADOR` - Can make purchases

**Default Users (Seed Data):**
- Admin: `admin@email.com.br` / `senha123`
- Buyer: `comprador@email.com.br` / `senha123`

### 2. **Produto** (Product Catalog)
```java
@Entity
public class Produto {
    @Id @GeneratedValue Long id;
    String nome;          // Unique
    String descricao;
    @Enumerated Categoria categoria;  // PET_FOOD, TOYS, ACCESSORIES, etc
    BigDecimal preco;
    Boolean ativo;        // Soft delete
}
```

**Supported Categories:**
- PET_FOOD (Pet food)
- TOYS (Toys)
- ACCESSORIES (Accessories)
- GROOMING (Grooming)
- FURNITURE (Pet furniture)

### 3. **Estoque** (Inventory Control)
```java
@Entity
public class Estoque {
    @Id @GeneratedValue Long id;
    @OneToOne Produto produto;
    Integer quantidade;   // Available quantity
}
```

### 4. **Pedido** (Customer Orders)
```java
@Entity
public class Pedido {
    @Id @GeneratedValue Long id;
    LocalDate data;
    @OneToMany List<ItemPedido> itens;  // Order items
    @ManyToOne Usuario usuario;         // Who bought
}
```

### 5. **ItemPedido** (Order Items)
```java
@Entity
public class ItemPedido {
    @Id @GeneratedValue Long id;
    @ManyToOne Produto produto;
    Integer quantidade;
    BigDecimal precoUnitario;
    @ManyToOne Pedido pedido;
}
```

### 6. **Perfil** (Access Roles)
```java
@Entity
public class Perfil implements GrantedAuthority {
    @Id @GeneratedValue Long id;
    String nome;  // ROLE_ADMIN, ROLE_COMPRADOR
}
```

---

## 🔌 API Endpoints

### 🔐 Authentication
| Method | Route | Description | Access |
|--------|-------|-------------|--------|
| POST | `/login` | Login and JWT generation | Public |

**Request:**
```json
{
  "email": "admin@email.com.br",
  "senha": "senha123"
}
```

**Response:**
```json
"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

---

### 📦 Products (Admin)
| Method | Route | Description | Access |
|--------|-------|-------------|--------|
| POST | `/admin/produtos` | Create new product | ADMIN |
| GET | `/admin/produtos` | List all (with pagination) | ADMIN |
| DELETE | `/admin/produtos/{id}` | Delete product | ADMIN |

**POST - Create Product:**
```json
{
  "nome": "Ração Premium Cat",
  "descricao": "Ração premium para gatos adultos",
  "categoria": "pet_food",
  "preco": 89.90
}
```

---

### 📊 Inventory (Admin)
| Method | Route | Description | Access |
|--------|-------|-------------|--------|
| GET | `/admin/estoques` | List all inventories | ADMIN |
| PUT | `/admin/estoques` | Update quantity | ADMIN |

**GET - List Inventories:**
```json
[
  {
    "id": 1,
    "produto": {...},
    "quantidade": 150
  }
]
```

**PUT - Update Inventory:**
```json
{
  "produtoId": 1,
  "quantidade": 200
}
```

---

### 🛒 Orders (Buyer)
| Method | Route | Description | Access |
|--------|-------|-------------|--------|
| POST | `/pedidos` | Create new order | COMPRADOR |

**POST - Create Order:**
```json
{
  "itens": [
    {
      "produtoId": 1,
      "quantidade": 2
    },
    {
      "produtoId": 3,
      "quantidade": 1
    }
  ]
}
```

**Response:**
```json
{
  "id": 1,
  "data": "2024-02-03",
  "itens": [...],
  "usuario": {...}
}
```

---

### 📈 Reports (Admin)
| Method | Route | Description | Access |
|--------|-------|-------------|--------|
| GET | `/admin/relatorios/estoque` | Products with zero inventory | ADMIN |
| GET | `/admin/relatorios/faturamento` | Revenue from previous day | ADMIN |

**GET - Inventory Report:**
```json
{
  "produtosSemEstoque": [
    {
      "id": 5,
      "nome": "Brinquedo Xis",
      "preco": 29.90
    }
  ]
}
```

**GET - Revenue Report:**
```json
{
  "faturamentoTotal": 1500.50,
  "porCategoria": [
    {
      "categoria": "PET_FOOD",
      "faturamento": 900.00
    },
    {
      "categoria": "TOYS",
      "faturamento": 600.50
    }
  ]
}
```

---

## 🔒 Authentication and Security

### JWT Authentication Flow

1. **Login**: Client sends email and password
2. **Validation**: Spring Security validates credentials
3. **Token**: If valid, `TokenService` generates JWT
4. **Return**: JWT is returned to the client
5. **Usage**: Client includes token in `Authorization: Bearer <token>` header
6. **Validation**: `SecurityFilter` validates token on each request

### Security Configurations

```java
// Pattern: STATELESS (no session)
// CSRF: Disabled (REST API)
// Authorization based on roles (RBAC)
```

**Access Hierarchy:**
```
Public
  └── /login

Authenticated
  └── /pedidos (POST) - COMPRADOR

Admin
  ├── /admin/produtos/** - ADMIN
  ├── /admin/estoques/** - ADMIN
  └── /admin/relatorios/** - ADMIN
```

### Password Encryption

- Algorithm: **BCrypt**
- Rounds: 10
- Automatic during registration/login

---

## ⚙️ Main Features

### 1. 🛍️ Product Management
- Create products with name, description, category, and price
- List products with pagination
- Deactivate products (soft delete)
- Category control (enum)

### 2. 📦 Inventory Control
- Automatic inventory when creating products
- Update available quantity
- Real-time validation
- Report on products with zero inventory

### 3. 🛒 Order Processing
- Inventory validations
- Automatic quantity reduction
- Support for multiple items per order
- Order history per user
- Validation of active products

### 4. 📧 Asynchronous Email Sending (Threads)
- **@Async**: Sends in separate thread
- Simulates sending with 3-second delay
- Order confirmation email
- Does not block HTTP request

```java
@Component
public class EnviadorEmail {
    @Async
    public void enviarEmail(String assunto, String destinatario, String texto) {
        Thread.sleep(3000);  // Simulates sending
        System.out.println("Email sent!");
    }
}
```

### 5. 📊 Intelligent Reports
- **Inventory**: Identifies critical products
- **Revenue**: Daily total + by category
- Foundation for sales analysis

### 6. 🔐 Authentication and Authorization
- JWT (JSON Web Tokens)
- Support for multiple roles
- Custom filter for validation
- Password encrypted with BCrypt

### 7. 💾 Database Versioning (Flyway)
- Automatic SQL migrations
- Schema versioning
- Initial seed data (default users)

---

## 🚀 Configuration and Installation

### Prerequisites

- **Java 21+** ([Download](https://www.oracle.com/java/technologies/downloads/#java21))
- **MySQL 8.0+** ([Download](https://www.mysql.com/downloads/))
- **Maven 3.8+** (included via mvnw)
- **Git** (optional, to clone repo)

### Step 1: Clone Repository

```bash
git clone https://github.com/seu-usuario/Pet-Store-Using-Java-Threads-And-Spring-Boot.git
cd Pet-Store-Using-Java-Threads-And-Spring-Boot
```

### Step 2: Configure Database

**Create MySQL database:**

```sql
CREATE DATABASE adopet_store;
CREATE USER 'root'@'localhost' IDENTIFIED BY 'root';
GRANT ALL PRIVILEGES ON adopet_store.* TO 'root'@'localhost';
FLUSH PRIVILEGES;
```

**Or use your own user/password and update in `application.properties`**

### Step 3: Configure Application

Edit `adopet-store/src/main/resources/application.properties`:

```properties
# Database
spring.datasource.url=jdbc:mysql://localhost/adopet_store?createDatabaseIfNotExist=true
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# JWT Secret (change for production security!)
app.security.token.secret=12345678

# Email (configure with your real credentials)
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=seu-email@gmail.com
spring.mail.password=sua-senha-app
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
```

### Step 4: Install Dependencies

```bash
cd adopet-store
./mvnw clean install
```

Or on Windows:
```bash
mvnw.cmd clean install
```

---

## ▶️ Running the Application

### Option 1: Via Maven

```bash
./mvnw spring-boot:run
```

### Option 2: After compiled

```bash
./mvnw package
java -jar target/adopet-store-0.0.1-SNAPSHOT.jar
```

### Option 3: IDE (IntelliJ IDEA)

1. Open the `adopet-store` folder as a Maven project
2. Right-click on `AdopetStoreApplication.java`
3. Select "Run" or press `Ctrl+Shift+F10`

### Verify if it's running

```bash
curl http://localhost:8080/login
```

Expected access at: **http://localhost:8080**

---

## 🔄 Workflow

### Complete Scenario: Making a Purchase

```
┌─────────────────────────────────────────────────────────────┐
│ 1. CLIENT LOGS IN                                           │
├─────────────────────────────────────────────────────────────┤
│ POST /login                                                  │
│ {                                                            │
│   "email": "comprador@email.com.br",                         │
│   "senha": "senha123"                                        │
│ }                                                            │
│                                                              │
│ Response: JWT Token                                          │
└─────────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│ 2. CLIENT SEES AVAILABLE PRODUCTS                           │
├─────────────────────────────────────────────────────────────┤
│ GET /admin/produtos (with JWT in header)                    │
│                                                              │
│ Response: Product list with inventory                        │
└─────────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│ 3. CLIENT PLACES ORDER                                      │
├─────────────────────────────────────────────────────────────┤
│ POST /pedidos (with JWT in header)                          │
│ {                                                            │
│   "itens": [                                                 │
│     { "produtoId": 1, "quantidade": 2 },                     │
│     { "produtoId": 3, "quantidade": 1 }                      │
│   ]                                                          │
│ }                                                            │
└─────────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│ 4. ORDER SERVICE VALIDATES                                  │
├─────────────────────────────────────────────────────────────┤
│ ✓ Inventory available?                                       │
│ ✓ Products active?                                           │
│ ✓ Reduce inventory                                           │
│ ✓ Create OrderItem                                           │
│ ✓ Save Order                                                 │
└─────────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│ 5. SEND EMAIL (ASYNC - Separate Thread)                     │
├─────────────────────────────────────────────────────────────┤
│ EnviadorEmail.enviarEmail()                                  │
│ └─ Thread.sleep(3000)  // Simulates sending                 │
│ └─ Prints confirmation                                       │
│                                                              │
│ ⚡ DOES NOT BLOCK HTTP response!                             │
└─────────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│ 6. RESPONSE TO CLIENT                                       │
├─────────────────────────────────────────────────────────────┤
│ HTTP 200 OK                                                  │
│ {                                                            │
│   "id": 1,                                                   │
│   "data": "2024-02-03",                                      │
│   "itens": [...],                                            │
│   "usuario": {...}                                           │
│ }                                                            │
└─────────────────────────────────────────────────────────────┘
```

### Order Validation Flow

```
Receive Order
      │
      ▼
For each order item
      │
      ├─▶ Exists in inventory?
      │   ├─ No ──▶ ❌ Error: Inventory unavailable
      │   └─ Yes ──┐
      │            │
      │            ▼
      │   Product is active?
      │   ├─ No ──▶ ❌ Error: Product deleted
      │   └─ Yes ──┐
      │            │
      │            ▼
      │   Reduce quantity
      │   └─ OK
      │
      ▼
All validated? ──▶ Yes
      │
      ▼
Create Order
      │
      ▼
Send Email (Async)
      │
      ▼
Return 200 OK
```

---

## 🧪 Testing

### Testing with cURL

**1. Login:**
```bash
curl -X POST http://localhost:8080/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@email.com.br","senha":"senha123"}'
```

**2. Create Product (save the previous token):**
```bash
curl -X POST http://localhost:8080/admin/produtos \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $TOKEN" \
  -d '{
    "nome":"Ração Gato",
    "descricao":"Premium",
    "categoria":"pet_food",
    "preco":89.90
  }'
```

**3. List Inventories:**
```bash
curl http://localhost:8080/admin/estoques \
  -H "Authorization: Bearer $TOKEN"
```

**4. Place Order (with buyer user):**
```bash
# First login as buyer
TOKEN=$(curl -s -X POST http://localhost:8080/login \
  -H "Content-Type: application/json" \
  -d '{"email":"comprador@email.com.br","senha":"senha123"}')

# Then place the order
curl -X POST http://localhost:8080/pedidos \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $TOKEN" \
  -d '{"itens":[{"produtoId":1,"quantidade":2}]}'
```

---

## 📝 Implementation Details

### Spring Data JPA
- **Automatic queries** in repositories
- **@Query** for custom queries
- **Pagination**: `Pageable` for listings
- **Lazy Loading**: FetchType.LAZY for optimization

### Spring Security
- **Stateless**: No session (ideal for API)
- **JWT**: Token in Authorization header
- **BCrypt**: Password hash with salt
- **@PreAuthorize**: Method-level control

### Async Processing
- **@Async**: Processes email in separate thread
- **@EnableAsync**: Enable asynchronous processing
- **Does not block**: Response returns immediately

### Validation
- **@Valid**: Validates DTO in request
- **@NotBlank**: Required fields
- **@Positive**: Positive values
- **Custom validators**: Custom validations

---

## 🐛 Troubleshooting

### Error: "Access denied to database"
```bash
# Check if MySQL is running
mysql -u root -p -e "SELECT 1;"

# Create database if it doesn't exist
mysql -u root -p -e "CREATE DATABASE adopet_store;"
```

### Error: "Port 8080 already in use"
```bash
# Linux/Mac: Find process
lsof -i :8080
kill -9 <PID>

# Windows: Via PowerShell
Get-Process -Name java | Stop-Process -Force
```

### Error: "Invalid JWT Token"
- Verify that the token is being sent in the header
- Confirm the format: `Authorization: Bearer <token>`
- Check if the token has not expired

### Email is not being sent
- Check credentials in `application.properties`
- Enable "App Passwords" if using Gmail
- Check logs: `EnviadorEmail` prints when trying to send

---

## 📚 References and Resources

- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Spring Data JPA](https://spring.io/projects/spring-data-jpa)
- [Spring Security](https://spring.io/projects/spring-security)
- [Java JWT](https://github.com/auth0/java-jwt)
- [Flyway](https://flywaydb.org/)
- [MySQL Documentation](https://dev.mysql.com/doc/)

---

## 📄 License

This project is provided as an educational example. Use it freely for learning and development.

---

## 👨‍💻 Author

**Developed during a Spring Boot and Threads course - Alura**

For questions or suggestions, open an issue in the repository.

---

## 🎓 Concepts Learned

This project demonstrates:
- ✅ Layered architecture (MVC)
- ✅ Spring Boot with Spring Data JPA
- ✅ Authentication and authorization (JWT + BCrypt)
- ✅ Asynchronous threads (@Async)
- ✅ Data validation (Jakarta Validation)
- ✅ Custom exception handling
- ✅ Database migrations with Flyway
- ✅ RESTful API design
- ✅ Security in REST APIs
- ✅ Pagination and performance
- ✅ Enums and Type-safe code
- ✅ DTOs and layer separation

---

**Last update:** February 2024
