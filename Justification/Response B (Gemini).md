This is the complete, high-performance architectural design and full-scale source implementation for the **Smart Expense Tracker** platform. The codebase incorporates explicit state synchronization, structural type-safety configurations, custom validation middleware, optimized schema relationships, and glassmorphism styling patterns.

# **1\. System Topology & Monolithic Folder Blueprints**

### **📂 Backend System Architecture (smart-expense-tracker-backend)**

Plaintext

```
smart-expense-tracker-backend/
├── pom.xml
└── src/
    └── main/
        ├── java/
        │   └── com/
        │       └── apex/
        │           └── fintech/
        │               ├── ExpenseTrackerApplication.java
        │               ├── config/
        │               │   ├── CorsConfig.java
        │               │   └── SecurityConfig.java
        │               ├── controller/
        │               │   ├── AuthController.java
        │               │   ├── CategoryController.java
        │               │   ├── DashboardController.java
        │               │   ├── ExpenseController.java
        │               │   └── IncomeController.java
        │               ├── dto/
        │               │   ├── ApiResponse.java
        │               │   ├── AuthRequest.java
        │               │   ├── AuthResponse.java
        │               │   ├── DashboardSummary.java
        │               │   ├── ExpenseDTO.java
        │               │   ├── IncomeDTO.java
        │               │   └── RegisterRequest.java
        │               ├── entity/
        │               │   ├── Category.java
        │               │   ├── Expense.java
        │               │   ├── Income.java
        │               │   └── User.java
        │               ├── exception/
        │               │   ├── ErrorResponse.java
        │               │   ├── GlobalExceptionHandler.java
        │               │   └── ResourceNotFoundException.java
        │               ├── repository/
        │               │   ├── CategoryRepository.java
        │               │   ├── ExpenseRepository.java
        │               │   ├── IncomeRepository.java
        │               │   └── UserRepository.java
        │               ├── security/
        │               │   ├── CustomUserDetailsService.java
        │               │   ├── JwtAuthenticationFilter.java
        │               │   └── JwtTokenProvider.java
        │               └── service/
        │                   ├── CategoryService.java
        │                   ├── DashboardService.java
        │                   ├── ExpenseService.java
        │                   └── IncomeService.java
        └── resources/
            └── application.yml
```

### **📂 Frontend Application Architecture (smart-expense-tracker-frontend)**

Plaintext

```
smart-expense-tracker-frontend/
├── package.json
├── tailwind.config.js
├── postcss.config.js
├── index.html
└── src/
    ├── main.jsx
    ├── index.css
    ├── context/
    │   └── AuthContext.jsx
    ├── services/
    │   └── api.js
    ├── components/
    │   ├── Layout.tsx
    │   ├── Sidebar.tsx
    │   ├── Navbar.tsx
    │   ├── StatCard.tsx
    │   ├── Modal.tsx
    │   ├── FormInput.tsx
    │   └── SelectInput.tsx
    └── pages/
        ├── Login.tsx
        ├── Signup.tsx
        ├── Dashboard.tsx
        ├── Expenses.tsx
        ├── Incomes.tsx
        ├── Categories.tsx
        ├── Analytics.tsx
        ├── Profile.tsx
        └── NotFound.tsx
```

# **2\. Backend Infrastructure & Core Configurations**

### **📄 Dependency Management Matrix: pom.xml**

XML

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.5</version>
        <relativePath/>
    </parent>
    <groupId>com.apex</groupId>
    <artifactId>expense-tracker</artifactId>
    <version>1.0.0</version>
    <name>expense-tracker</name>
    <description>Production Ready Smart Expense Tracker Corporate Core API</description>

    <properties>
        <java.version>17</java.version>
        <jjwt.version>0.11.5</jjwt.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt-api</artifactId>
            <version>${jjwt.version}</version>
        </dependency>
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt-impl</artifactId>
            <version>${jjwt.version}</version>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt-jackson</artifactId>
            <version>${jjwt.version}</version>
            <scope>runtime</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

### **⚙️ Production Application Strategy Configuration: application.yml**

YAML

```
server:
  port: 8080
  error:
    include-message: always

spring:
  datasource:
    url: jdbc:mysql://${DB_HOST:localhost}:${DB_PORT:3306}/${DB_NAME:expense_db}?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true
    username: ${DB_USER:root}
    password: ${DB_PASSWORD:password}
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: false
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQLDialect
        format_sql: true

app:
  jwt:
    secret: 404E635266556A586E3272357538782F413F4428472B4B6250645367566B5970404E635266556A586E3272357538782F413F4428472B4B6250645367566B5970
    expiration-ms: 86400000
```

# **3\. Normalized Database Entity Model Framework**

### **👤 User.java**

Java

```
package com.apex.fintech.entity;

import jakarta.persistence.*;
import lombok.*;
import java.time.LocalDateTime;

@Entity
@Table(name = "users", uniqueConstraints = {@UniqueConstraint(columnNames = "email")})
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, length = 100)
    private String name;

    @Column(nullable = false, length = 100, unique = true)
    private String email;

    @Column(nullable = false, length = 255)
    private String password;

    @Column(name = "created_at", updatable = false)
    private LocalDateTime createdAt;

    @PrePersist
    protected void onCreate() {
        createdAt = LocalDateTime.now();
    }
}
```

### **🏷️ Category.java**

Java

```
package com.apex.fintech.entity;

import jakarta.persistence.*;
import lombok.*;

@Entity
@Table(name = "categories")
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class Category {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, length = 50)
    private String name;

    @Column(length = 7)
    private String color;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id", nullable = false)
    private User user;
}
```

### **📉 Expense.java**

Java

```
package com.apex.fintech.entity;

import jakarta.persistence.*;
import lombok.*;
import java.math.BigDecimal;
import java.time.LocalDate;

@Entity
@Table(name = "expenses")
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class Expense {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, length = 150)
    private String title;

    @Column(nullable = false, precision = 12, scale = 2)
    private BigDecimal amount;

    @Column(nullable = false)
    private LocalDate date;

    @Column(length = 500)
    private String description;

    @ManyToOne(fetch = FetchType.EAGER)
    @JoinColumn(name = "category_id", nullable = false)
    private Category category;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id", nullable = false)
    private User user;
}
```

### **📈 Income.java**

Java

```
package com.apex.fintech.entity;

import jakarta.persistence.*;
import lombok.*;
import java.math.BigDecimal;
import java.time.LocalDate;

@Entity
@Table(name = "incomes")
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class Income {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, length = 150)
    private String source;

    @Column(nullable = false, precision = 12, scale = 2)
    private BigDecimal amount;

    @Column(nullable = false)
    private LocalDate date;

    @Column(length = 500)
    private String description;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id", nullable = false)
    private User user;
}
```

# **4\. Persistence Repository Layers**

Java

```
package com.apex.fintech.repository;

import com.apex.fintech.entity.User;
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.Optional;

public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
    boolean existsByEmail(String email);
}
```

Java

```
package com.apex.fintech.repository;

import com.apex.fintech.entity.Category;
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.List;

public interface CategoryRepository extends JpaRepository<Category, Long> {
    List<Category> findByUserId(Long userId);
}
```

Java

```
package com.apex.fintech.repository;

import com.apex.fintech.entity.Expense;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;
import java.util.List;

public interface ExpenseRepository extends JpaRepository<Expense, Long> {
    List<Expense> findByUserIdOrderByDateDesc(Long userId);
    
    @Query("SELECT e FROM Expense e WHERE e.user.id = :userId AND (:categoryId IS NULL OR e.category.id = :categoryId) ORDER BY e.date DESC")
    List<Expense> filterExpenses(@Param("userId") Long userId, @Param("categoryId") Long categoryId);
}
```

Java

```
package com.apex.fintech.repository;

import com.apex.fintech.entity.Income;
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.List;

public interface IncomeRepository extends JpaRepository<Income, Long> {
    List<Income> findByUserIdOrderByDateDesc(Long userId);
}
```

# **5\. Enterprise Security & JWT Management**

### **🔑 JwtTokenProvider.java**

Java

```
package com.apex.fintech.security;

import io.jsonwebtoken.*;
import io.jsonwebtoken.security.Keys;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;
import java.security.Key;
import java.util.Date;

@Component
public class JwtTokenProvider {

    private final Key key;
    private final long jwtExpirationInMs;

    public JwtTokenProvider(
            @Value("${app.jwt.secret}") String jwtSecret,
            @Value("${app.jwt.expiration-ms}") long jwtExpirationInMs) {
        this.key = Keys.hmacShaKeyFor(jwtSecret.getBytes());
        this.jwtExpirationInMs = jwtExpirationInMs;
    }

    public String generateToken(String email) {
        Date now = new Date();
        Date expiryDate = new Date(now.getTime() + jwtExpirationInMs);

        return Jwts.builder()
                .setSubject(email)
                .setIssuedAt(now)
                .setExpiration(expiryDate)
                .signWith(key, SignatureAlgorithm.HS256)
                .compact();
    }

    public String getEmailFromJWT(String token) {
        Claims claims = Jwts.parserBuilder()
                .setSigningKey(key)
                .build()
                .parseClaimsJws(token)
                .getBody();
        return claims.getSubject();
    }

    public boolean validateToken(String authToken) {
        try {
            Jwts.parserBuilder().setSigningKey(key).build().parseClaimsJws(authToken);
            return true;
        } catch (JwtException | IllegalArgumentException ex) {
            return false;
        }
    }
}
```

### **🔍 JwtAuthenticationFilter.java**

Java

```
package com.apex.fintech.security;

import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import lombok.RequiredArgsConstructor;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.stereotype.Component;
import org.springframework.util.StringUtils;
import org.springframework.web.filter.OncePerRequestFilter;
import java.io.IOException;

@Component
@RequiredArgsConstructor
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    private final JwtTokenProvider tokenProvider;
    private final CustomUserDetailsService userDetailsService;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {
        try {
            String jwt = getJwtFromRequest(request);

            if (StringUtils.hasText(jwt) && tokenProvider.validateToken(jwt)) {
                String username = tokenProvider.getEmailFromJWT(jwt);
                UserDetails userDetails = userDetailsService.loadUserByUsername(username);
                
                UsernamePasswordAuthenticationToken authentication = 
                    new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
                authentication.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));

                SecurityContextHolder.getContext().setAuthentication(authentication);
            }
        } catch (Exception ex) {
            logger.error("Could not set user authentication in security context", ex);
        }

        filterChain.doFilter(request, response);
    }

    private String getJwtFromRequest(HttpServletRequest request) {
        String bearerToken = request.getHeader("Authorization");
        if (StringUtils.hasText(bearerToken) && bearerToken.startsWith("Bearer ")) {
            return bearerToken.substring(7);
        }
        return null;
    }
}
```

### **👤 CustomUserDetailsService.java**

Java

```
package com.apex.fintech.security;

import com.apex.fintech.entity.User;
import com.apex.fintech.repository.UserRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.security.core.userdetails.*;
import org.springframework.stereotype.Service;
import java.util.Collections;

@Service
@RequiredArgsConstructor
public class CustomUserDetailsService implements UserDetailsService {

    private final UserRepository userRepository;

    @Override
    public UserDetails loadUserByUsername(String email) throws UsernameNotFoundException {
        User user = userRepository.findByEmail(email)
                .orElseThrow(() -> new UsernameNotFoundException("User not found with email: " + email));

        return new org.springframework.security.core.userdetails.User(
                user.getEmail(), 
                user.getPassword(), 
                Collections.emptyList()
        );
    }
}
```

### **🛡️ SecurityConfig.java**

Java

```
package com.apex.fintech.config;

import com.apex.fintech.security.JwtAuthenticationFilter;
import lombok.RequiredArgsConstructor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configurers.AbstractHttpConfigurer;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
@EnableWebSecurity
@RequiredArgsConstructor
public class SecurityConfig {

    private final JwtAuthenticationFilter jwtAuthenticationFilter;

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration config) throws Exception {
        return config.getAuthenticationManager();
    }

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf(AbstractHttpConfigurer::disable)
            .cors(cors -> cors.configure(http))
            .sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**").permitAll()
                .anyRequest().authenticated()
            );

        http.addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class);
        return http.build();
    }
}
```

### **🌐 CorsConfig.java**

Java

```
package com.apex.fintech.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class CorsConfig {

    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**")
                        .allowedOriginPatterns("*")
                        .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                        .allowedHeaders("*")
                        .allowCredentials(true);
            }
        };
    }
}
```

# **6\. Data Transfer Object (DTO) Declarations**

Java

```
package com.apex.fintech.dto;

import jakarta.validation.constraints.*;
import lombok.Data;

@Data
public class RegisterRequest {
    @NotBlank(message = "Name field cannot be left unassigned")
    private String name;

    @NotBlank(message = "Email configuration coordinate required")
    @Email(message = "Malformed tracking identity structural notation")
    private String email;

    @NotBlank(message = "Alphanumeric cryptographic token sequence required")
    @Size(min = 6, message = "Cipher sequence must contain 6 or more segments")
    private String password;
}
```

Java

```
package com.apex.fintech.dto;

import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.NotBlank;
import lombok.Data;

@Data
public class AuthRequest {
    @NotBlank(message = "Account key identification target required")
    @Email(message = "Structural identity validation criteria failed")
    private String email;

    @NotBlank(message = "Authorization key assertion required")
    private String password;
}
```

Java

```
package com.apex.fintech.dto;

import lombok.AllArgsConstructor;
import lombok.Data;

@AllArgsConstructor
@Data
public class AuthResponse {
    private String token;
    private String email;
    private String name;
}
```

Java

```
package com.apex.fintech.dto;

import lombok.Builder;
import lombok.Data;
import java.math.BigDecimal;
import java.util.List;
import java.util.Map;

@Data
@Builder
public class DashboardSummary {
    private BigDecimal totalIncome;
    private BigDecimal totalExpenses;
    private BigDecimal remainingBalance;
    private Map<String, BigDecimal> categoryDistribution;
    private List<?> recentTransactions;
}
```

Java

```
package com.apex.fintech.dto;

import lombok.Data;
import java.math.BigDecimal;
import java.time.LocalDate;

@Data
public class ExpenseDTO {
    private Long id;
    private String title;
    private BigDecimal amount;
    private LocalDate date;
    private String description;
    private Long categoryId;
}
```

Java

```
package com.apex.fintech.dto;

import lombok.Data;
import java.math.BigDecimal;
import java.time.LocalDate;

@Data
public class IncomeDTO {
    private Long id;
    private String source;
    private BigDecimal amount;
    private LocalDate date;
    private String description;
}
```

Java

```
package com.apex.fintech.dto;

import lombok.AllArgsConstructor;
import lombok.Data;

@Data
@AllArgsConstructor
public class ApiResponse<T> {
    private boolean success;
    private String message;
    private T data;
}
```

# **7\. Core Business Application Services**

### **📊 DashboardService.java**

Java

```
package com.apex.fintech.service;

import com.apex.fintech.dto.DashboardSummary;
import com.apex.fintech.entity.Expense;
import com.apex.fintech.entity.Income;
import com.apex.fintech.entity.User;
import com.apex.fintech.repository.ExpenseRepository;
import com.apex.fintech.repository.IncomeRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

@Service
@RequiredArgsConstructor
public class DashboardService {

    private final ExpenseRepository expenseRepository;
    private final IncomeRepository incomeRepository;

    public DashboardSummary getDashboardMetrics(User user) {
        List<Expense> expenses = expenseRepository.findByUserIdOrderByDateDesc(user.getId());
        List<Income> incomes = incomeRepository.findByUserIdOrderByDateDesc(user.getId());

        BigDecimal totalIncome = incomes.stream()
                .map(Income::getAmount)
                .reduce(BigDecimal.ZERO, BigDecimal::add);

        BigDecimal totalExpense = expenses.stream()
                .map(Expense::getAmount)
                .reduce(BigDecimal.ZERO, BigDecimal::add);

        Map<String, BigDecimal> categoryDistribution = expenses.stream()
                .collect(Collectors.groupingBy(
                        e -> e.getCategory().getName(),
                        Collectors.mapping(Expense::getAmount, Collectors.reducing(BigDecimal.ZERO, BigDecimal::add))
                ));

        List<Object> combinedTransactions = new ArrayList<>();
        combinedTransactions.addAll(expenses);
        combinedTransactions.addAll(incomes);
        
        combinedTransactions.sort((b, a) -> {
            java.time.LocalDate dateA = (a instanceof Expense) ? ((Expense) a).getDate() : ((Income) a).getDate();
            java.time.LocalDate dateB = (b instanceof Expense) ? ((Expense) b).getDate() : ((Income) b).getDate();
            return dateA.compareTo(dateB);
        });

        List<?> recent = combinedTransactions.stream().limit(5).collect(Collectors.toList());

        return DashboardSummary.builder()
                .totalIncome(totalIncome)
                .totalExpenses(totalExpense)
                .remainingBalance(totalIncome.subtract(totalExpense))
                .categoryDistribution(categoryDistribution)
                .recentTransactions(recent)
                .build();
    }
}
```

### **📉 ExpenseService.java**

Java

```
package com.apex.fintech.service;

import com.apex.fintech.dto.ExpenseDTO;
import com.apex.fintech.entity.Category;
import com.apex.fintech.entity.Expense;
import com.apex.fintech.entity.User;
import com.apex.fintech.exception.ResourceNotFoundException;
import com.apex.fintech.repository.CategoryRepository;
import com.apex.fintech.repository.ExpenseRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
@RequiredArgsConstructor
public class ExpenseService {

    private final ExpenseRepository expenseRepository;
    private final CategoryRepository categoryRepository;

    public List<Expense> getFilteredExpenses(User user, Long categoryId) {
        return expenseRepository.filterExpenses(user.getId(), categoryId);
    }

    public Expense createExpense(ExpenseDTO dto, User user) {
        Category category = categoryRepository.findById(dto.getCategoryId())
                .orElseThrow(() -> new ResourceNotFoundException("Target metric tracking classification node missing"));
        
        Expense expense = Expense.builder()
                .title(dto.getTitle())
                .amount(dto.getAmount())
                .date(dto.getDate())
                .description(dto.getDescription())
                .category(category)
                .user(user)
                .build();
                
        return expenseRepository.save(expense);
    }

    public Expense updateExpense(Long id, ExpenseDTO dto, User user) {
        Expense expense = expenseRepository.findById(id)
                .filter(e -> e.getUser().getId().equals(user.getId()))
                .orElseThrow(() -> new ResourceNotFoundException("Target operational structural trace ledger entry missing"));
        
        Category category = categoryRepository.findById(dto.getCategoryId())
                .orElseThrow(() -> new ResourceNotFoundException("Classification container target modification identity missing"));

        expense.setTitle(dto.getTitle());
        expense.setAmount(dto.getAmount());
        expense.setDate(dto.getDate());
        expense.setDescription(dto.getDescription());
        expense.setCategory(category);
        
        return expenseRepository.save(expense);
    }

    public void deleteExpense(Long id, User user) {
        Expense expense = expenseRepository.findById(id)
                .filter(e -> e.getUser().getId().equals(user.getId()))
                .orElseThrow(() -> new ResourceNotFoundException("Deletion context validation requirements failed"));
        expenseRepository.delete(expense);
    }
}
```

### **📈 IncomeService.java**

Java

```
package com.apex.fintech.service;

import com.apex.fintech.dto.IncomeDTO;
import com.apex.fintech.entity.Income;
import com.apex.fintech.entity.User;
import com.apex.fintech.exception.ResourceNotFoundException;
import com.apex.fintech.repository.IncomeRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
@RequiredArgsConstructor
public class IncomeService {

    private final IncomeRepository incomeRepository;

    public List<Income> getAllIncomes(User user) {
        return incomeRepository.findByUserIdOrderByDateDesc(user.getId());
    }

    public Income createIncome(IncomeDTO dto, User user) {
        Income income = Income.builder()
                .source(dto.getSource())
                .amount(dto.getAmount())
                .date(dto.getDate())
                .description(dto.getDescription())
                .user(user)
                .build();
        return incomeRepository.save(income);
    }

    public Income updateIncome(Long id, IncomeDTO dto, User user) {
        Income income = incomeRepository.findById(id)
                .filter(i -> i.getUser().getId().equals(user.getId()))
                .orElseThrow(() -> new ResourceNotFoundException("Target income node execution trace context invalid"));
        
        income.setSource(dto.getSource());
        income.setAmount(dto.getAmount());
        income.setDate(dto.getDate());
        income.setDescription(dto.getDescription());
        
        return incomeRepository.save(income);
    }

    public void deleteIncome(Long id, User user) {
        Income income = incomeRepository.findById(id)
                .filter(i -> i.getUser().getId().equals(user.getId()))
                .orElseThrow(() -> new ResourceNotFoundException("Revenue ledger removal lifecycle validation failed"));
        incomeRepository.delete(income);
    }
}
```

### **🏷️ CategoryService.java**

Java

```
package com.apex.fintech.service;

import com.apex.fintech.entity.Category;
import com.apex.fintech.entity.User;
import com.apex.fintech.repository.CategoryRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import java.util.Arrays;
import java.util.List;

@Service
@RequiredArgsConstructor
public class CategoryService {

    private final CategoryRepository categoryRepository;

    public List<Category> getCategoriesByUser(User user) {
        List<Category> categories = categoryRepository.findByUserId(user.getId());
        if (categories.isEmpty()) {
            return seedDefaultCategories(user);
        }
        return categories;
    }

    public Category createCategory(Category category, User user) {
        category.setUser(user);
        return categoryRepository.save(category);
    }

    private List<Category> seedDefaultCategories(User user) {
        List<Category> defaultCategories = Arrays.asList(
                Category.builder().name("Food").color("#EF4444").user(user).build(),
                Category.builder().name("Travel").color("#3B82F6").user(user).build(),
                Category.builder().name("Shopping").color("#F59E0B").user(user).build(),
                Category.builder().name("Bills").color("#10B981").user(user).build(),
                Category.builder().name("Entertainment").color("#8B5CF6").user(user).build(),
                Category.builder().name("Health").color("#EC4899").user(user).build()
        );
        return categoryRepository.saveAll(defaultCategories);
    }
}
```

# **8\. Web MVC Controller Endpoints**

### **👤 AuthController.java**

Java

```
package com.apex.fintech.controller;

import com.apex.fintech.dto.*;
import com.apex.fintech.entity.User;
import com.apex.fintech.repository.UserRepository;
import com.apex.fintech.security.JwtTokenProvider;
import jakarta.validation.Valid;
import lombok.RequiredArgsConstructor;
import org.springframework.http.ResponseEntity;
import org.springframework.security.authentication.*;
import org.springframework.security.core.Authentication;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/auth")
@RequiredArgsConstructor
public class AuthController {

    private final AuthenticationManager authenticationManager;
    private final UserRepository userRepository;
    private final PasswordEncoder passwordEncoder;
    private final JwtTokenProvider tokenProvider;

    @PostMapping("/signup")
    public ResponseEntity<ApiResponse<AuthResponse>> registerUser(@Valid @RequestBody RegisterRequest registerRequest) {
        if (userRepository.existsByEmail(registerRequest.getEmail())) {
            return ResponseEntity.badRequest().body(new ApiResponse<>(false, "Dynamic database error: Identifier identity profile conflict.", null));
        }

        User user = User.builder()
                .name(registerRequest.getName())
                .email(registerRequest.getEmail())
                .password(passwordEncoder.encode(registerRequest.getPassword()))
                .build();

        userRepository.save(user);
        String token = tokenProvider.generateToken(user.getEmail());
        return ResponseEntity.ok(new ApiResponse<>(true, "Profile generated successfully", new AuthResponse(token, user.getEmail(), user.getName())));
    }

    @PostMapping("/login")
    public ResponseEntity<ApiResponse<AuthResponse>> authenticateUser(@Valid @RequestBody AuthRequest loginRequest) {
        Authentication authentication = authenticationManager.authenticate(
                new UsernamePasswordAuthenticationToken(loginRequest.getEmail(), loginRequest.getPassword())
        );

        User user = userRepository.findByEmail(loginRequest.getEmail()).orElseThrow();
        String token = tokenProvider.generateToken(user.getEmail());
        return ResponseEntity.ok(new ApiResponse<>(true, "Authorization metrics validation accepted", new AuthResponse(token, user.getEmail(), user.getName())));
    }
}
```

### **📊 DashboardController.java**

Java

```
package com.apex.fintech.controller;

import com.apex.fintech.dto.ApiResponse;
import com.apex.fintech.dto.DashboardSummary;
import com.apex.fintech.entity.User;
import com.apex.fintech.repository.UserRepository;
import com.apex.fintech.service.DashboardService;
import lombok.RequiredArgsConstructor;
import org.springframework.http.ResponseEntity;
import org.springframework.security.core.annotation.AuthenticationPrincipal;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/dashboard")
@RequiredArgsConstructor
public class DashboardController {

    private final DashboardService dashboardService;
    private final UserRepository userRepository;

    @GetMapping
    public ResponseEntity<ApiResponse<DashboardSummary>> getDashboardSummary(@AuthenticationPrincipal UserDetails userDetails) {
        User user = userRepository.findByEmail(userDetails.getUsername()).orElseThrow();
        return ResponseEntity.ok(new ApiResponse<>(true, "Metrics synchronizations complete", dashboardService.getDashboardMetrics(user)));
    }
}
```

### **📉 ExpenseController.java**

Java

```
package com.apex.fintech.controller;

import com.apex.fintech.dto.ApiResponse;
import com.apex.fintech.dto.ExpenseDTO;
import com.apex.fintech.entity.Expense;
import com.apex.fintech.entity.User;
import com.apex.fintech.repository.UserRepository;
import com.apex.fintech.service.ExpenseService;
import jakarta.validation.Valid;
import lombok.RequiredArgsConstructor;
import org.springframework.http.ResponseEntity;
import org.springframework.security.core.annotation.AuthenticationPrincipal;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/api/expenses")
@RequiredArgsConstructor
public class ExpenseController {

    private final ExpenseService expenseService;
    private final UserRepository userRepository;

    @GetMapping
    public ResponseEntity<ApiResponse<List<Expense>>> getExpenses(
            @AuthenticationPrincipal UserDetails userDetails,
            @RequestParam(required = false) Long categoryId) {
        User user = userRepository.findByEmail(userDetails.getUsername()).orElseThrow();
        return ResponseEntity.ok(new ApiResponse<>(true, "Liability data matrices loaded", expenseService.getFilteredExpenses(user, categoryId)));
    }

    @PostMapping
    public ResponseEntity<ApiResponse<Expense>> createExpense(@AuthenticationPrincipal UserDetails userDetails, @Valid @RequestBody ExpenseDTO dto) {
        User user = userRepository.findByEmail(userDetails.getUsername()).orElseThrow();
        return ResponseEntity.ok(new ApiResponse<>(true, "Liability segment tracked successfully", expenseService.createExpense(dto, user)));
    }

    @PutMapping("/{id}")
    public ResponseEntity<ApiResponse<Expense>> updateExpense(@AuthenticationPrincipal UserDetails userDetails, @PathVariable Long id, @Valid @RequestBody ExpenseDTO dto) {
        User user = userRepository.findByEmail(userDetails.getUsername()).orElseThrow();
        return ResponseEntity.ok(new ApiResponse<>(true, "Ledger trace matrix state modification applied", expenseService.updateExpense(id, dto, user)));
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<ApiResponse<Void>> deleteExpense(@AuthenticationPrincipal UserDetails userDetails, @PathVariable Long id) {
        User user = userRepository.findByEmail(userDetails.getUsername()).orElseThrow();
        expenseService.deleteExpense(id, user);
        return ResponseEntity.ok(new ApiResponse<>(true, "Trace context successfully truncated from persistence logs", null));
    }
}
```

### **📈 IncomeController.java**

Java

```
package com.apex.fintech.controller;

import com.apex.fintech.dto.ApiResponse;
import com.apex.fintech.dto.IncomeDTO;
import com.apex.fintech.entity.Income;
import com.apex.fintech.entity.User;
import com.apex.fintech.repository.UserRepository;
import com.apex.fintech.service.IncomeService;
import jakarta.validation.Valid;
import lombok.RequiredArgsConstructor;
import org.springframework.http.ResponseEntity;
import org.springframework.security.core.annotation.AuthenticationPrincipal;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/api/incomes")
@RequiredArgsConstructor
public class IncomeController {

    private final IncomeService incomeService;
    private final UserRepository userRepository;

    @GetMapping
    public ResponseEntity<ApiResponse<List<Income>>> getIncomes(@AuthenticationPrincipal UserDetails userDetails) {
        User user = userRepository.findByEmail(userDetails.getUsername()).orElseThrow();
        return ResponseEntity.ok(new ApiResponse<>(true, "Asset entry configuration trace loaded", incomeService.getAllIncomes(user)));
    }

    @PostMapping
    public ResponseEntity<ApiResponse<Income>> createIncome(@AuthenticationPrincipal UserDetails userDetails, @Valid @RequestBody IncomeDTO dto) {
        User user = userRepository.findByEmail(userDetails.getUsername()).orElseThrow();
        return ResponseEntity.ok(new ApiResponse<>(true, "Capital allocation entry mapped", incomeService.createIncome(dto, user)));
    }

    @PutMapping("/{id}")
    public ResponseEntity<ApiResponse<Income>> updateIncome(@AuthenticationPrincipal UserDetails userDetails, @PathVariable Long id, @Valid @RequestBody IncomeDTO dto) {
        User user = userRepository.findByEmail(userDetails.getUsername()).orElseThrow();
        return ResponseEntity.ok(new ApiResponse<>(true, "Revenue distribution coordinate altered", incomeService.updateIncome(id, dto, user)));
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<ApiResponse<Void>> deleteIncome(@AuthenticationPrincipal UserDetails userDetails, @PathVariable Long id) {
        User user = userRepository.findByEmail(userDetails.getUsername()).orElseThrow();
        incomeService.deleteIncome(id, user);
        return ResponseEntity.ok(new ApiResponse<>(true, "Target asset metric point deleted", null));
    }
}
```

### **🏷️ CategoryController.java**

Java

```
package com.apex.fintech.controller;

import com.apex.fintech.dto.ApiResponse;
import com.apex.fintech.entity.Category;
import com.apex.fintech.entity.User;
import com.apex.fintech.repository.UserRepository;
import com.apex.fintech.service.CategoryService;
import jakarta.validation.Valid;
import lombok.RequiredArgsConstructor;
import org.springframework.http.ResponseEntity;
import org.springframework.security.core.annotation.AuthenticationPrincipal;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/api/categories")
@RequiredArgsConstructor
public class CategoryController {

    private final CategoryService categoryService;
    private final UserRepository userRepository;

    @GetMapping
    public ResponseEntity<ApiResponse<List<Category>>> getCategories(@AuthenticationPrincipal UserDetails userDetails) {
        User user = userRepository.findByEmail(userDetails.getUsername()).orElseThrow();
        return ResponseEntity.ok(new ApiResponse<>(true, "Custom classification maps extracted", categoryService.getCategoriesByUser(user)));
    }

    @PostMapping
    public ResponseEntity<ApiResponse<Category>> createCategory(@AuthenticationPrincipal UserDetails userDetails, @Valid @RequestBody Category category) {
        User user = userRepository.findByEmail(userDetails.getUsername()).orElseThrow();
        return ResponseEntity.ok(new ApiResponse<>(true, "Structural parsing categorization node created", categoryService.createCategory(category, user)));
    }
}
```

# **9\. Application Resilience & Exception Framework**

Java

```
package com.apex.fintech.exception;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ResponseStatus;

@ResponseStatus(HttpStatus.NOT_FOUND)
public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String message) {
        super(message);
    }
}
```

Java

```
package com.apex.fintech.exception;

import lombok.AllArgsConstructor;
import lombok.Data;
import java.time.LocalDateTime;

@Data
@AllArgsConstructor
public class ErrorResponse {
    private LocalDateTime timestamp;
    private int status;
    private String error;
    private String message;
}
```

Java

```
package com.apex.fintech.exception;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import java.time.LocalDateTime;

@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleNotFound(ResourceNotFoundException ex) {
        ErrorResponse err = new ErrorResponse(LocalDateTime.now(), HttpStatus.NOT_FOUND.value(), "Not Found Map", ex.getMessage());
        return new ResponseEntity<>(err, HttpStatus.NOT_FOUND);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGeneric(Exception ex) {
        ErrorResponse err = new ErrorResponse(LocalDateTime.now(), HttpStatus.INTERNAL_SERVER_ERROR.value(), "Internal Strategy Failure", ex.getMessage());
        return new ResponseEntity<>(err, HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

# **10\. Frontend Configuration & UI Styles**

### **📄 Project Manifest Declaration: package.json**

JSON

```
{
  "name": "smart-expense-tracker-frontend",
  "private": true,
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "axios": "^1.6.8",
    "lucide-react": "^0.379.0",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-router-dom": "^6.23.1",
    "recharts": "^2.12.7"
  },
  "devDependencies": {
    "@types/react": "^18.3.3",
    "@types/react-dom": "^18.3.0",
    "@vitejs/plugin-react": "^4.3.0",
    "autoprefixer": "^10.4.19",
    "postcss": "^8.4.38",
    "tailwindcss": "^3.4.3",
    "vite": "^5.2.11"
  }
}
```

### **🎨 Design System Utility Tokens: tailwind.config.js**

JavaScript

```
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        fintech: {
          darkBg: '#090D16',
          panel: 'rgba(13, 20, 35, 0.65)',
          accent: '#4F46E5',
          emeraldNode: '#10B981',
          crimsonNode: '#F43F5E'
        }
      },
      backdropBlur: {
        premium: '24px',
      }
    },
  },
  plugins: [],
}
```

### **🎛️ Design Injection Matrix: src/index.css**

CSS

```
@tailwindcss base;
@tailwindcss components;
@tailwindcss utilities;

body {
  margin: 0;
  background: radial-gradient(circle at 80% 10%, #1E1B4B 0%, #070A12 70%);
  color: #F8FAFC;
  font-family: 'Inter', sans-serif;
  min-height: 100vh;
  overflow-x: hidden;
}

.glass-panel {
  background: rgba(15, 23, 42, 0.45);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 255, 255, 0.05);
}

.glass-modal {
  background: rgba(10, 15, 30, 0.9);
  backdrop-filter: blur(24px);
  border: 1px solid rgba(255, 255, 255, 0.08);
}

::-webkit-scrollbar {
  width: 6px;
}
::-webkit-scrollbar-track {
  background: transparent;
}
::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.08);
  border-radius: 999px;
}
::-webkit-scrollbar-thumb:hover {
  background: rgba(79, 70, 229, 0.4);
}
```

# **11\. Core State Operations & Network Middleware**

### **🔄 Client Session Verification Engine: src/context/AuthContext.jsx**

JavaScript

```
import React, { createContext, useState, useEffect, useContext } from 'react';

const AuthContext = createContext(null);

export const AuthProvider = ({ children }) => {
    const [user, setUser] = useState(() => {
        const activeProfile = localStorage.getItem('apex_session');
        return activeProfile ? JSON.parse(activeProfile) : null;
    });

    const login = (userData) => {
        setUser(userData);
        localStorage.setItem('apex_session', JSON.stringify(userData));
    };

    const logout = () => {
        setUser(null);
        localStorage.removeItem('apex_session');
    };

    return (
        <AuthContext.Provider value={{ user, login, logout, isAuthenticated: !!user }}>
            {children}
        </AuthContext.Provider>
    );
};

export const useAuth = () => useContext(AuthContext);
```

### **🌐 Axios Client Interceptor Configuration: src/services/api.js**

JavaScript

```
import axios from 'axios';

const API = axios.create({
    baseURL: 'http://localhost:8080/api',
});

API.interceptors.request.use((config) => {
    const session = localStorage.getItem('apex_session');
    if (session) {
        const { token } = JSON.parse(session);
        config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
}, (error) => {
    return Promise.reject(error);
});

export const authAPI = {
    login: (payload) => API.post('/auth/login', payload),
    signup: (payload) => API.post('/auth/signup', payload),
};

export const dashboardAPI = {
    getSummary: () => API.get('/dashboard'),
};

export const expenseAPI = {
    getAll: (catId) => API.get('/expenses', { params: { categoryId: catId } }),
    create: (payload) => API.post('/expenses', payload),
    update: (id, payload) => API.put(`/expenses/${id}`, payload),
    delete: (id) => API.delete(`/expenses/${id}`),
};

export const incomeAPI = {
    getAll: () => API.get('/incomes'),
    create: (payload) => API.post('/incomes', payload),
    update: (id, payload) => API.put(`/incomes/${id}`, payload),
    delete: (id) => API.delete(`/incomes/${id}`),
};

export const categoryAPI = {
    getAll: () => API.get('/categories'),
    create: (payload) => API.post('/categories', payload),
};

export default API;
```

# **12\. Main Application Ingress & Layout Shells**

### **🗺️ Client Layout Routing Map: src/main.jsx**

JavaScript

```
import React from 'react';
import ReactDOM from 'react-dom/client';
import { BrowserRouter, Routes, Route, Navigate } from 'react-router-dom';
import { AuthProvider, useAuth } from './context/AuthContext';
import Layout from './components/Layout';
import Login from './pages/Login';
import Signup from './pages/Signup';
import Dashboard from './pages/Dashboard';
import Expenses from './pages/Expenses';
import Incomes from './pages/Incomes';
import Categories from './pages/Categories';
import Analytics from './pages/Analytics';
import Profile from './pages/Profile';
import NotFound from './pages/NotFound';
import './index.css';

const Gatekeeper = ({ children }) => {
    const { isAuthenticated } = useAuth();
    return isAuthenticated ? children : <Navigate to="/login" replace />;
};

ReactDOM.createRoot(document.getElementById('root')).render(
    <AuthProvider>
        <BrowserRouter>
            <Routes>
                <Route path="/login" element={<Login />} />
                <Route path="/signup" element={<Signup />} />
                
                <Route path="/" element={<Gatekeeper><Layout /></Gatekeeper>}>
                    <Route index element={<Navigate to="/dashboard" replace />} />
                    <Route path="dashboard" element={<Dashboard />} />
                    <Route path="expenses" element={<Expenses />} />
                    <Route path="incomes" element={<Incomes />} />
                    <Route path="categories" element={<Categories />} />
                    <Route path="analytics" element={<Analytics />} />
                    <Route path="profile" element={<Profile />} />
                </Route>

                <Route path="*" element={<NotFound />} />
            </Routes>
        </BrowserRouter>
    </AuthProvider>
);
```

### **🎚️ Application Viewport Frame Component: src/components/Layout.tsx**

JavaScript

```
import React from 'react';
import { Outlet } from 'react-router-dom';
import Sidebar from './Sidebar';
import Navbar from './Navbar';

const Layout = () => {
    return (
        <div className="flex min-h-screen overflow-hidden bg-transparent">
            <Sidebar />
            <div className="flex-1 flex flex-col min-w-0 overflow-hidden">
                <Navbar />
                <main className="flex-1 overflow-y-auto p-6 lg:p-10 max-w-7xl w-full mx-auto">
                    <Outlet />
                </main>
            </div>
        </div>
    );
};

export default Layout;
```

# **13\. Reusable Atomic UI Component Framework**

### **🧭 src/components/Sidebar.tsx**

JavaScript

```
import React from 'react';
import { NavLink } from 'react-router-dom';
import { LayoutDashboard, ArrowDownCircle, ArrowUpCircle, Layers, BarChart3, User, Wallet } from 'lucide-react';

const Sidebar = () => {
    const navItems = [
        { path: '/dashboard', icon: LayoutDashboard, label: 'Terminal Core' },
        { path: '/expenses', icon: ArrowDownCircle, label: 'Outbound Ledger' },
        { path: '/incomes', icon: ArrowUpCircle, label: 'Inbound Asset Flow' },
        { path: '/categories', icon: Layers, label: 'Schema Mappings' },
        { path: '/analytics', icon: BarChart3, label: 'Analytics Cluster' },
        { path: '/profile', icon: User, label: 'Node Profile' },
    ];

    return (
        <aside className="hidden lg:flex flex-col w-64 bg-slate-950/40 backdrop-blur-premium border-r border-slate-900 p-6 z-20">
            <div className="flex items-center gap-3 mb-10 px-2">
                <Wallet className="h-7 w-7 text-indigo-500" />
                <span className="text-lg font-bold tracking-wider bg-gradient-to-r from-white to-slate-400 bg-clip-text text-transparent">
                    APEX FI
                </span>
            </div>
            <nav className="space-y-1.5 flex-1">
                {navItems.map((item) => {
                    const Icon = item.icon;
                    return (
                        <NavLink
                            key={item.path}
                            to={item.path}
                            className={({ isActive }) => `flex items-center gap-3.5 px-4 py-3 rounded-xl text-sm font-medium transition-all duration-200 ${
                                isActive 
                                    ? 'bg-indigo-600 text-white shadow-lg shadow-indigo-600/15 border-l-2 border-indigo-400' 
                                    : 'text-slate-400 hover:bg-slate-900/50 hover:text-white'
                            }`}
                        >
                            <Icon className="h-4 w-4" />
                            {item.label}
                        </NavLink>
                    );
                })}
            </nav>
        </aside>
    );
};

export default Sidebar;
```

### **🎛️ src/components/Navbar.tsx**

JavaScript

```
import React from 'react';
import { useAuth } from '../context/AuthContext';
import { LogOut, User as UserIcon } from 'lucide-react';

const Navbar = () => {
    const { user, logout } = useAuth();

    return (
        <header className="h-20 border-b border-slate-900/60 glass-panel flex items-center justify-between px-8 z-10">
            <div className="flex flex-col">
                <span className="text-xs text-slate-500 uppercase font-mono tracking-widest">Platform Operational Console</span>
                <span className="text-sm font-medium text-slate-300">Welcome, {user?.name || 'Operator'}</span>
            </div>
            <div className="flex items-center gap-4">
                <div className="flex items-center gap-2.5 bg-slate-950/40 border border-slate-800 rounded-xl px-3 py-1.5">
                    <UserIcon className="h-4 w-4 text-indigo-400" />
                    <span className="text-xs font-mono text-slate-400">{user?.email}</span>
                </div>
                <button 
                    onClick={logout}
                    className="p-2.5 bg-slate-950/20 hover:bg-rose-500/10 border border-slate-800 hover:border-rose-500/30 text-slate-400 hover:text-rose-400 rounded-xl transition-all"
                    title="Terminate Session Allocation"
                >
                    <LogOut className="h-4 w-4" />
                </button>
            </div>
        </header>
    );
};

export default Navbar;
```

### **📊 src/components/StatCard.tsx**

JavaScript

```
import React from 'react';
import { LucideIcon } from 'lucide-react';

interface StatProps {
    title: string;
    value: number;
    icon: LucideIcon;
    colorClass: string;
}

const StatCard: React.FC<StatProps> = ({ title, value, icon: Icon, colorClass }) => {
    return (
        <div className="glass-panel rounded-2xl p-6 shadow-2xl relative overflow-hidden group hover:border-slate-800 transition-all">
            <div className="flex items-center justify-between mb-4">
                <span className="text-xs font-mono text-slate-400 uppercase tracking-wider">{title}</span>
                <div className={`p-2.5 rounded-xl bg-slate-950/40 border border-slate-800 ${colorClass}`}>
                    <Icon className="h-4 w-4" />
                </div>
            </div>
            <h3 className="text-2xl font-bold tracking-tight text-white">
                ${parseFloat(value?.toString() || '0').toLocaleString('en-US', { minimumFractionDigits: 2 })}
            </h3>
        </div>
    );
};

export default StatCard;
```

### **📦 src/components/Modal.tsx**

JavaScript

```
import React from 'react';
import { X } from 'lucide-react';

interface ModalProps {
    isOpen: boolean;
    onClose: () => void;
    title: string;
    children: React.ReactNode;
}

const Modal: React.FC<ModalProps> = ({ isOpen, onClose, title, children }) => {
    if (!isOpen) return null;

    return (
        <div className="fixed inset-0 z-50 flex items-center justify-center p-4">
            <div className="absolute inset-0 bg-black/70 backdrop-blur-sm" onClick={onClose} />
            <div className="glass-modal rounded-2xl w-full max-w-lg relative z-10 p-6 shadow-2xl max-h-[90vh] overflow-y-auto">
                <div className="flex justify-between items-center mb-6 border-b border-slate-800/60 pb-3">
                    <h3 className="text-base font-semibold text-white tracking-wide">{title}</h3>
                    <button onClick={onClose} className="p-1 rounded-lg hover:bg-slate-900 text-slate-400 hover:text-white transition-colors">
                        <X className="h-4 w-4" />
                    </button>
                </div>
                {children}
            </div>
        </div>
    );
};

export default Modal;
```

### **📥 src/components/FormInput.tsx**

JavaScript

```
import React from 'react';

interface InputProps extends React.InputHTMLAttributes<HTMLInputElement> {
    label: string;
}

const FormInput: React.FC<InputProps> = ({ label, ...props }) => {
    return (
        <div>
            <label className="block text-xs font-mono uppercase tracking-wider text-slate-400 mb-2">{label}</label>
            <input 
                {...props}
                className="w-full bg-slate-950/40 border border-slate-800 rounded-xl px-4 py-2.5 text-sm text-white focus:outline-none focus:border-indigo-500 transition-colors"
            />
        </div>
    );
};

export default FormInput;
```

### **🗃️ src/components/SelectInput.tsx**

JavaScript

```
import React from 'react';

interface SelectProps extends React.SelectHTMLAttributes<HTMLSelectElement> {
    label: string;
    options: { value: string | number; label: string }[];
}

const SelectInput: React.FC<SelectProps> = ({ label, options, ...props }) => {
    return (
        <div>
            <label className="block text-xs font-mono uppercase tracking-wider text-slate-400 mb-2">{label}</label>
            <select 
                {...props}
                className="w-full bg-slate-950/40 border border-slate-800 rounded-xl px-4 py-2.5 text-sm text-white focus:outline-none focus:border-indigo-500 transition-colors appearance-none"
            >
                <option value="" className="bg-slate-950">Select execution parameter...</option>
                {options.map((opt) => (
                    <option key={opt.value} value={opt.value} className="bg-slate-950">{opt.label}</option>
                ))}
            </select>
        </div>
    );
};

export default SelectInput;
```

# **14\. Core Functional Dashboard Pages**

### **🔐 src/pages/Login.tsx**

JavaScript

```
import React, { useState } from 'react';
import { Link, useNavigate } from 'react-router-dom';
import { authAPI } from '../services/api';
import { useAuth } from '../context/AuthContext';
import { Wallet } from 'lucide-react';
import FormInput from '../components/FormInput';

const Login = () => {
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');
    const [error, setError] = useState('');
    const [loading, setLoading] = useState(false);
    const { login } = useAuth();
    const navigate = useNavigate();

    const handleAuthenticationSubmit = async (e: React.FormEvent) => {
        e.preventDefault();
        setError('');
        setLoading(true);
        try {
            const response = await authAPI.login({ email, password });
            if (response.data.success) {
                login(response.data.data);
                navigate('/dashboard');
            } else {
                setError(response.data.message);
            }
        } catch (err: any) {
            setError(err.response?.data?.message || 'Identity validation protocols aborted.');
        } finally {
            setLoading(false);
        }
    };

    return (
        <div className="min-h-screen flex items-center justify-center p-4">
            <div className="w-full max-w-md glass-panel rounded-2xl p-8 shadow-2xl">
                <div className="flex flex-col items-center mb-8">
                    <div className="p-3 bg-indigo-600/10 border border-indigo-500/20 rounded-xl mb-3">
                        <Wallet className="h-6 w-6 text-indigo-500" />
                    </div>
                    <h2 className="text-xl font-bold tracking-tight text-white">Access Identity Token</h2>
                    <p className="text-xs text-slate-500 mt-1">Synchronize session with centralized server metrics</p>
                </div>

                {error && <div className="mb-4 p-3 bg-rose-500/10 border border-rose-500/20 text-rose-400 rounded-xl text-xs font-mono">{error}</div>}

                <form onSubmit={handleAuthenticationSubmit} className="space-y-4">
                    <FormInput label="Identification String" type="email" required placeholder="operator@apex.fi" value={email} onChange={e => setEmail(e.target.value)} />
                    <FormInput label="Security Credentials" type="password" required placeholder="••••••••" value={password} onChange={e => setPassword(e.target.value)} />
                    
                    <button 
                        type="submit" disabled={loading}
                        className="w-full py-2.5 mt-2 bg-indigo-600 hover:bg-indigo-500 text-white rounded-xl text-sm font-medium transition-all shadow-lg shadow-indigo-600/10 disabled:opacity-40"
                    >
                        {loading ? 'Validating Token Matrices...' : 'Connect to Console'}
                    </button>
                </form>
                <div className="mt-6 text-center text-xs text-slate-500">
                    No active node address? <Link to="/signup" className="text-indigo-400 hover:underline">Deploy personal container</Link>
                </div>
            </div>
        </div>
    );
};

export default Login;
```

### **🛡️ src/pages/Signup.tsx**

JavaScript

```
import React, { useState } from 'react';
import { Link, useNavigate } from 'react-router-dom';
import { authAPI } from '../services/api';
import { useAuth } from '../context/AuthContext';
import { Wallet } from 'lucide-react';
import FormInput from '../components/FormInput';

const Signup = () => {
    const [name, setName] = useState('');
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');
    const [error, setError] = useState('');
    const [loading, setLoading] = useState(false);
    const { login } = useAuth();
    const navigate = useNavigate();

    const handleRegistrationSubmit = async (e: React.FormEvent) => {
        e.preventDefault();
        setError('');
        setLoading(true);
        try {
            const response = await authAPI.signup({ name, email, password });
            if (response.data.success) {
                login(response.data.data);
                navigate('/dashboard');
            } else {
                setError(response.data.message);
            }
        } catch (err: any) {
            setError(err.response?.data?.message || 'Failed processing initialization parameters.');
        } finally {
            setLoading(false);
        }
    };

    return (
        <div className="min-h-screen flex items-center justify-center p-4">
            <div className="w-full max-w-md glass-panel rounded-2xl p-8 shadow-2xl">
                <div className="flex flex-col items-center mb-8">
                    <div className="p-3 bg-indigo-600/10 border border-indigo-500/20 rounded-xl mb-3">
                        <Wallet className="h-6 w-6 text-indigo-500" />
                    </div>
                    <h2 className="text-xl font-bold tracking-tight text-white">Initialize Platform Instance</h2>
                    <p className="text-xs text-slate-500 mt-1">Configure workspace allocation mappings</p>
                </div>

                {error && <div className="mb-4 p-3 bg-rose-500/10 border border-rose-500/20 text-rose-400 rounded-xl text-xs font-mono">{error}</div>}

                <form onSubmit={handleRegistrationSubmit} className="space-y-4">
                    <FormInput label="Operator Name" type="text" required placeholder="John Doe" value={name} onChange={e => setName(e.target.value)} />
                    <FormInput label="Operational Identification Node (Email)" type="email" required placeholder="operator@apex.fi" value={email} onChange={e => setEmail(e.target.value)} />
                    <FormInput label="Cryptographic Password Configuration" type="password" required placeholder="••••••••" value={password} onChange={e => setPassword(e.target.value)} />
                    
                    <button 
                        type="submit" disabled={loading}
                        className="w-full py-2.5 mt-2 bg-indigo-600 hover:bg-indigo-500 text-white rounded-xl text-sm font-medium transition-all shadow-lg shadow-indigo-600/10 disabled:opacity-40"
                    >
                        {loading ? 'Provisioning Infrastructure Space...' : 'Instantiate System'}
                    </button>
                </form>
                <div className="mt-6 text-center text-xs text-slate-500">
                    Existing active account verified? <Link to="/login" className="text-indigo-400 hover:underline">Re-route to terminal access</Link>
                </div>
            </div>
        </div>
    );
};

export default Signup;
```

### **📊 src/pages/Dashboard.tsx**

JavaScript

```
import React, { useEffect, useState } from 'react';
import { dashboardAPI } from '../services/api';
import StatCard from '../components/StatCard';
import { ArrowDownCircle, ArrowUpCircle, Wallet, Activity, Terminal } from 'lucide-react';
import { ResponsiveContainer, AreaChart, Area, XAxis, YAxis, Tooltip, CartesianGrid } from 'recharts';

const Dashboard = () => {
    const [summary, setSummary] = useState<any>(null);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
        dashboardAPI.getSummary()
            .then((res) => { if (res.data.success) setSummary(res.data.data); })
            .catch((err) => console.error('Dashboard analytical error configuration trace:', err))
            .finally(() => setLoading(false));
    }, []);

    if (loading) return <div className="h-64 flex items-center justify-center text-xs font-mono text-slate-500">Executing system evaluation trace logs...</div>;

    const chartData = summary ? Object.entries(summary.categoryDistribution).map(([name, value]) => ({ name, value })) : [];

    return (
        <div className="space-y-8">
            <div>
                <h1 className="text-2xl font-bold tracking-tight text-white">Operational Dashboard Overview</h1>
                <p className="text-slate-500 text-xs mt-1">Real-time compilation of verified account assets and liabilities</p>
            </div>

            <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
                <StatCard title="Aggregated Income Inflow" value={summary?.totalIncome} icon={ArrowUpCircle} colorClass="text-emerald-400" />
                <StatCard title="Aggregated Liability Outflow" value={summary?.totalExpenses} icon={ArrowDownCircle} colorClass="text-rose-400" />
                <StatCard title="Net Liquid Metric Evaluations" value={summary?.remainingBalance} icon={Wallet} colorClass="text-indigo-400" />
            </div>

            <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
                <div className="lg:col-span-2 glass-panel rounded-2xl p-6 shadow-xl">
                    <h3 className="text-xs font-mono uppercase tracking-wider text-slate-400 mb-6">Structural Cost Distributions Vector</h3>
                    <div className="h-64">
                        {chartData.length > 0 ? (
                            <ResponsiveContainer width="100%" height="100%">
                                <AreaChart data={chartData}>
                                    <defs>
                                        <linearGradient id="indigoFade" x1="0" y1="0" x2="0" y2="1">
                                            <stop offset="5%" stopColor="#4F46E5" stopOpacity={0.25}/>
                                            <stop offset="95%" stopColor="#4F46E5" stopOpacity={0}/>
                                        </linearGradient>
                                    </defs>
                                    <CartesianGrid strokeDasharray="3 3" stroke="#131B2E" vertical={false} />
                                    <XAxis dataKey="name" stroke="#475569" fontSize={11} fontFamily="monospace" tickLine={false} />
                                    <YAxis stroke="#475569" fontSize={11} fontFamily="monospace" tickLine={false} axisLine={false} />
                                    <Tooltip contentStyle={{ backgroundColor: '#090D16', border: '1px solid #1E293B', borderRadius: '12px', fontSize: '12px' }} />
                                    <Area type="monotone" dataKey="value" stroke="#4F46E5" strokeWidth={2} fillOpacity={1} fill="url(#indigoFade)" />
                                </AreaChart>
                            </ResponsiveContainer>
                        ) : (
                            <div className="h-full flex items-center justify-center text-xs font-mono text-slate-600">No categoric profiles matched to active metrics</div>
                        )}
                    </div>
                </div>

                <div className="glass-panel rounded-2xl p-6 shadow-xl flex flex-col">
                    <div className="flex items-center gap-2 mb-6 border-b border-slate-800 pb-3">
                        <Activity className="h-4 w-4 text-indigo-400" />
                        <h3 className="text-xs font-mono uppercase tracking-wider text-slate-400">System Log Stream</h3>
                    </div>
                    <div className="space-y-3 flex-1 overflow-y-auto max-h-64 pr-1">
                        {summary?.recentTransactions?.map((tx: any, idx: number) => {
                            const isExpense = 'category' in tx;
                            return (
                                <div key={idx} className="flex justify-between items-center bg-slate-950/40 border border-slate-900 rounded-xl p-3">
                                    <div className="min-w-0 flex-1 pr-3">
                                        <p className="text-xs font-medium text-slate-200 truncate">{tx.title || tx.source}</p>
                                        <p className="text-[10px] font-mono text-slate-500 mt-0.5">{tx.date}</p>
                                    </div>
                                    <span className={`text-xs font-mono font-semibold ${isExpense ? 'text-rose-400' : 'text-emerald-400'}`}>
                                        {isExpense ? '-' : '+'}${tx.amount}
                                    </span>
                                </div>
                            );
                        })}
                        {summary?.recentTransactions?.length === 0 && (
                            <div className="h-full flex items-center justify-center text-xs font-mono text-slate-600 py-12">System operational history log clean</div>
                        )}
                    </div>
                </div>
            </div>
        </div>
    );
};

export default Dashboard;
```

### **📉 src/pages/Expenses.tsx**

JavaScript

```
import React, { useEffect, useState } from 'react';
import { expenseAPI, categoryAPI } from '../services/api';
import Modal from '../components/Modal';
import FormInput from '../components/FormInput';
import SelectInput from '../components/SelectInput';
import { Plus, Trash2, SlidersHorizontal, Search } from 'lucide-react';

const Expenses = () => {
    const [expenses, setExpenses] = useState([]);
    const [categories, setCategories] = useState([]);
    const [loading, setLoading] = useState(true);
    const [isOpen, setIsOpen] = useState(false);
    
    const [title, setTitle] = useState('');
    const [amount, setAmount] = useState('');
    const [date, setDate] = useState('');
    const [description, setDescription] = useState('');
    const [categoryId, setCategoryId] = useState('');
    const [filterCategory, setFilterCategory] = useState('');
    const [searchTerm, setSearchTerm] = useState('');

    const synchronizeDataState = () => {
        setLoading(true);
        Promise.all([expenseAPI.getAll(filterCategory), categoryAPI.getAll()])
            .then(([expRes, catRes]) => {
                if (expRes.data.success) setExpenses(expRes.data.data);
                if (catRes.data.success) setCategories(catRes.data.data);
            })
            .catch(err => console.error(err))
            .finally(() => setLoading(false));
    };

    useEffect(() => { synchronizeDataState(); }, [filterCategory]);

    const handleCreateExecution = (e: React.FormEvent) => {
        e.preventDefault();
        const payload = { title, amount: parseFloat(amount), date, description, categoryId: parseInt(categoryId) };
        expenseAPI.create(payload).then(() => {
            setIsOpen(false);
            synchronizeDataState();
            setTitle(''); setAmount(''); setDate(''); setDescription(''); setCategoryId('');
        });
    };

    const handleDeleteTrack = (id: number) => {
        if (confirm("Confirm database row purge protocol?")) {
            expenseAPI.delete(id).then(() => synchronizeDataState());
        }
    };

    const filteredExpensesList = expenses.filter((exp: any) => 
        exp.title.toLowerCase().includes(searchTerm.toLowerCase())
    );

    return (
        <div className="space-y-6">
            <div className="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4">
                <div>
                    <h1 className="text-2xl font-bold text-white">Outbound Liability Logs</h1>
                    <p className="text-slate-500 text-xs mt-0.5">Audit configuration matrix mapping internal expense streams</p>
                </div>
                <button 
                    onClick={() => setIsOpen(true)}
                    className="flex items-center gap-2 bg-indigo-600 hover:bg-indigo-500 text-white text-xs font-mono uppercase px-4 py-2.5 rounded-xl transition-all shadow-lg shadow-indigo-600/10 self-start"
                >
                    <Plus className="h-4 w-4" /> Inject Matrix Record
                </button>
            </div>

            <div className="grid grid-cols-1 md:grid-cols-3 gap-4 bg-slate-950/20 border border-slate-900 rounded-2xl p-4">
                <div className="relative">
                    <Search className="absolute left-3.5 top-3 h-4 w-4 text-slate-500" />
                    <input 
                        type="text" placeholder="Filter strings..."
                        className="w-full bg-slate-950/40 border border-slate-850 rounded-xl pl-10 pr-4 py-2 text-xs text-white focus:outline-none focus:border-indigo-500"
                        value={searchTerm} onChange={e => setSearchTerm(e.target.value)}
                    />
                </div>
                <div className="relative">
                    <SlidersHorizontal className="absolute left-3.5 top-3 h-4 w-4 text-slate-500" />
                    <select
                        className="w-full bg-slate-950/40 border border-slate-850 rounded-xl pl-10 pr-4 py-2 text-xs text-white focus:outline-none focus:border-indigo-500 appearance-none"
                        value={filterCategory} onChange={e => setFilterCategory(e.target.value)}
                    >
                        <option value="">All Category Maps</option>
                        {categories.map((c: any) => <option key={c.id} value={c.id}>{c.name}</option>)}
                    </select>
                </div>
            </div>

            {loading ? <div className="text-center font-mono text-xs text-slate-500 py-12">Parsing storage logs...</div> : (
                <div className="glass-panel rounded-2xl overflow-hidden shadow-xl border border-slate-900/60">
                    <div className="overflow-x-auto">
                        <table className="w-full text-left border-collapse">
                            <thead>
                                <tr className="border-b border-slate-900 bg-slate-950/40 text-[11px] font-mono uppercase tracking-wider text-slate-400">
                                    <th className="p-4">Resource Target Label</th>
                                    <th className="p-4">Classification Group</th>
                                    <th className="p-4">Timestamp Node</th>
                                    <th className="p-4 text-right">Volume Value</th>
                                    <th className="p-4 text-center">Execution Action</th>
                                </tr>
                            </thead>
                            <tbody className="divide-y divide-slate-900/40 text-xs text-slate-300">
                                {filteredExpensesList.map((exp: any) => (
                                    <tr key={exp.id} className="hover:bg-slate-900/20 transition-colors">
                                        <td className="p-4 font-medium text-white">{exp.title}</td>
                                        <td className="p-4">
                                            <span className="inline-flex items-center gap-1.5 px-2.5 py-0.5 rounded-full text-[10px] font-mono bg-slate-950/60 border border-slate-800" style={{borderColor: exp.category?.color + '25'}}>
                                                <span className="w-1.5 h-1.5 rounded-full" style={{backgroundColor: exp.category?.color || '#4F46E5'}} />
                                                {exp.category?.name}
                                            </span>
                                        </td>
                                        <td className="p-4 font-mono text-slate-400">{exp.date}</td>
                                        <td className="p-4 text-right font-mono font-semibold text-rose-400">-${exp.amount}</td>
                                        <td className="p-4 text-center">
                                            <button onClick={() => handleDeleteTrack(exp.id)} className="text-slate-500 hover:text-rose-400 p-1 rounded-lg transition-colors">
                                                <Trash2 className="h-4 w-4" />
                                            </button>
                                        </td>
                                    </tr>
                                ))}
                                {filteredExpensesList.length === 0 && (
                                    <tr>
                                        <td colSpan={5} className="text-center py-12 font-mono text-slate-600">No execution traces match parameters</td>
                                    </tr>
                                )}
                            </tbody>
                        </table>
                    </div>
                </div>
            )}

            <Modal isOpen={isOpen} onClose={() => setIsOpen(false)} title="Track Outbound Asset Clearance">
                <form onSubmit={handleCreateExecution} className="space-y-4">
                    <FormInput label="Descriptor Key" type="text" required placeholder="AWS Cloud Cluster Invoicing" value={title} onChange={e => setTitle(e.target.value)} />
                    <div className="grid grid-cols-2 gap-4">
                        <FormInput label="Volume Metric ($)" type="number" step="0.01" required placeholder="0.00" value={amount} onChange={e => setAmount(e.target.value)} />
                        <FormInput label="Temporal Node Date" type="date" required value={date} onChange={e => setDate(e.target.value)} />
                    </div>
                    <SelectInput 
                        label="Mapping Matrix Classification" required value={categoryId} onChange={e => setCategoryId(e.target.value)}
                        options={categories.map((c: any) => ({ value: c.id, label: c.name }))}
                    />
                    <div>
                        <label className="block text-xs font-mono uppercase tracking-wider text-slate-400 mb-2">Meta Layer Notes</label>
                        <textarea className="w-full bg-slate-950/40 border border-slate-800 rounded-xl px-4 py-2.5 text-sm text-white focus:outline-none focus:border-indigo-400 min-h-24 resize-none" value={description} onChange={e => setDescription(e.target.value)} />
                    </div>
                    <button type="submit" className="w-full py-2.5 bg-indigo-600 hover:bg-indigo-500 text-white font-mono text-xs uppercase tracking-wider rounded-xl transition-all">
                        Commit Configuration State
                    </button>
                </form>
            </Modal>
        </div>
    );
};

export default Expenses;
```

### **📈 src/pages/Incomes.tsx**

JavaScript

```
import React, { useEffect, useState } from 'react';
import { incomeAPI } from '../services/api';
import Modal from '../components/Modal';
import FormInput from '../components/FormInput';
import { Plus, Trash2 } from 'lucide-react';

const Incomes = () => {
    const [incomes, setIncomes] = useState([]);
    const [loading, setLoading] = useState(true);
    const [isOpen, setIsOpen] = useState(false);
    
    const [source, setSource] = useState('');
    const [amount, setAmount] = useState('');
    const [date, setDate] = useState('');
    const [description, setDescription] = useState('');

    const fetchIncomeDataMatrix = () => {
        setLoading(true);
        incomeAPI.getAll()
            .then(res => { if (res.data.success) setIncomes(res.data.data); })
            .catch(err => console.error(err))
            .finally(() => setLoading(false));
    };

    useEffect(() => { fetchIncomeDataMatrix(); }, []);

    const handleCreateSubmit = (e: React.FormEvent) => {
        e.preventDefault();
        const payload = { source, amount: parseFloat(amount), date, description };
        incomeAPI.create(payload).then(() => {
            setIsOpen(false);
            fetchIncomeDataMatrix();
            setSource(''); setAmount(''); setDate(''); setDescription('');
        });
    };

    const handleDeleteIncomeTrack = (id: number) => {
        if (confirm("Execute deletion logic across persistent ledger storage?")) {
            incomeAPI.delete(id).then(() => fetchIncomeDataMatrix());
        }
    };

    return (
        <div className="space-y-6">
            <div className="flex justify-between items-center">
                <div>
                    <h1 className="text-2xl font-bold text-white">Inbound Revenue Vectors</h1>
                    <p className="text-slate-500 text-xs mt-0.5">Document incoming financial streams</p>
                </div>
                <button 
                    onClick={() => setIsOpen(true)}
                    className="flex items-center gap-2 bg-indigo-600 hover:bg-indigo-500 text-white text-xs font-mono uppercase px-4 py-2.5 rounded-xl transition-all shadow-lg shadow-indigo-600/10"
                >
                    <Plus className="h-4 w-4" /> Inject Capital Stream
                </button>
            </div>

            {loading ? <div className="text-center font-mono text-xs text-slate-500 py-12">Querying partition arrays...</div> : (
                <div className="glass-panel rounded-2xl overflow-hidden shadow-xl border border-slate-900/60">
                    <div className="overflow-x-auto">
                        <table className="w-full text-left border-collapse">
                            <thead>
                                <tr className="border-b border-slate-900 bg-slate-950/40 text-[11px] font-mono uppercase tracking-wider text-slate-400">
                                    <th className="p-4">Inflow Identity Source</th>
                                    <th className="p-4">Temporal Node Mapping</th>
                                    <th className="p-4 text-right">Volume Value</th>
                                    <th className="p-4 text-center">Execution Action</th>
                                </tr>
                            </thead>
                            <tbody className="divide-y divide-slate-900/40 text-xs text-slate-300">
                                {incomes.map((inc: any) => (
                                    <tr key={inc.id} className="hover:bg-slate-900/20 transition-colors">
                                        <td className="p-4 font-medium text-white">{inc.source}</td>
                                        <td className="p-4 font-mono text-slate-400">{inc.date}</td>
                                        <td className="p-4 text-right font-mono font-semibold text-emerald-400">+${inc.amount}</td>
                                        <td className="p-4 text-center">
                                            <button onClick={() => handleDeleteIncomeTrack(inc.id)} className="text-slate-500 hover:text-rose-400 p-1 rounded-lg transition-colors">
                                                <Trash2 className="h-4 w-4" />
                                            </button>
                                        </td>
                                    </tr>
                                ))}
                                {incomes.length === 0 && (
                                    <tr>
                                        <td colSpan={4} className="text-center py-12 font-mono text-slate-600">No inward allocations logged to memory matrix</td>
                                    </tr>
                                )}
                            </tbody>
                        </table>
                    </div>
                </div>
            )}

            <Modal isOpen={isOpen} onClose={() => setIsOpen(false)} title="Register Inbound Capital Assets">
                <form onSubmit={handleCreateSubmit} className="space-y-4">
                    <FormInput label="Revenue Pipeline Channel" type="text" required placeholder="SaaS Subscription Recurring Inflow" value={source} onChange={e => setSource(e.target.value)} />
                    <div className="grid grid-cols-2 gap-4">
                        <FormInput label="Volume ($)" type="number" step="0.01" required placeholder="0.00" value={amount} onChange={e => setAmount(e.target.value)} />
                        <FormInput label="Temporal Node Value" type="date" required value={date} onChange={e => setDate(e.target.value)} />
                    </div>
                    <div>
                        <label className="block text-xs font-mono uppercase tracking-wider text-slate-400 mb-2">Meta Context Layer Notes</label>
                        <textarea className="w-full bg-slate-950/40 border border-slate-800 rounded-xl px-4 py-2.5 text-sm text-white focus:outline-none focus:border-indigo-400 min-h-24 resize-none" value={description} onChange={e => setDescription(e.target.value)} />
                    </div>
                    <button type="submit" className="w-full py-2.5 bg-indigo-600 hover:bg-indigo-500 text-white font-mono text-xs uppercase tracking-wider rounded-xl transition-all">
                        Commit Fluid Income Channel
                    </button>
                </form>
            </Modal>
        </div>
    );
};

export default Incomes;
```

### **🏷️ src/pages/Categories.tsx**

JavaScript

```
import React, { useEffect, useState } from 'react';
import { categoryAPI } from '../services/api';
import FormInput from '../components/FormInput';
import { Plus } from 'lucide-react';

const Categories = () => {
    const [categories, setCategories] = useState([]);
    const [name, setName] = useState('');
    const [color, setColor] = useState('#4F46E5');

    const loadCategoryInfrastructureMap = () => {
        categoryAPI.getAll().then(res => { if (res.data.success) setCategories(res.data.data); });
    };

    useEffect(() => { loadCategoryInfrastructureMap(); }, []);

    const handleFormSubmit = (e: React.FormEvent) => {
        e.preventDefault();
        categoryAPI.create({ name, color }).then(() => {
            setName('');
            loadCategoryInfrastructureMap();
        });
    };

    return (
        <div className="space-y-8">
            <div>
                <h1 className="text-2xl font-bold text-white">System Schema Structural Mappings</h1>
                <p className="text-slate-500 text-xs mt-0.5">Configure categorical parameters for internal classification layers</p>
            </div>

            <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
                <div className="glass-panel rounded-2xl p-6 shadow-xl h-fit">
                    <h3 className="text-xs font-mono uppercase tracking-wider text-slate-400 mb-4">Inject Classification Parameter</h3>
                    <form onSubmit={handleFormSubmit} className="space-y-4">
                        <FormInput label="Group Allocation Label" type="text" required value={name} onChange={e => setName(e.target.value)} placeholder="Operational Overheads" />
                        <div>
                            <label className="block text-xs font-mono uppercase tracking-wider text-slate-400 mb-2">Visual Mapping Vector Hex Color</label>
                            <div className="flex gap-3">
                                <input type="color" value={color} className="w-12 h-10 bg-transparent border-0 cursor-pointer" onChange={e => setColor(e.target.value)} />
                                <input type="text" value={color} className="flex-1 bg-slate-950/40 border border-slate-800 rounded-xl px-4 text-xs font-mono text-white uppercase" readOnly />
                            </div>
                        </div>
                        <button type="submit" className="w-full py-2.5 bg-indigo-600 hover:bg-indigo-500 text-white font-mono text-xs uppercase tracking-wider rounded-xl flex items-center justify-center gap-2 transition-all">
                            <Plus className="h-4 w-4" /> Mount Vector
                        </button>
                    </form>
                </div>

                <div className="lg:col-span-2 grid grid-cols-2 sm:grid-cols-3 gap-4">
                    {categories.map((cat: any) => (
                        <div key={cat.id} className="bg-slate-950/20 border border-slate-900 rounded-2xl p-4 flex items-center gap-4 shadow-md">
                            <div className="w-3.5 h-3.5 rounded-full flex-shrink-0 shadow-sm" style={{ backgroundColor: cat.color || '#4F46E5' }} />
                            <span className="font-medium text-slate-200 text-xs truncate">{cat.name}</span>
                        </div>
                    ))}
                </div>
            </div>
        </div>
    );
};

export default Categories;
```

### **📊 src/pages/Analytics.tsx**

JavaScript

```
import React, { useEffect, useState } from 'react';
import { dashboardAPI } from '../services/api';
import { ResponsiveContainer, BarChart, Bar, XAxis, YAxis, Tooltip, CartesianGrid, Legend, PieChart, Pie, Cell } from 'recharts';

const Analytics = () => {
    const [analytics, setAnalytics] = useState<any>(null);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
        dashboardAPI.getSummary()
            .then(res => { if (res.data.success) setAnalytics(res.data.data); })
            .catch(err => console.error(err))
            .finally(() => setLoading(false));
    }, []);

    if (loading) return <div className="text-center font-mono text-xs text-slate-500 py-12">Compiling structural analytical profiles...</div>;

    const distributionData = analytics ? Object.entries(analytics.categoryDistribution).map(([name, value]) => ({ name, value })) : [];
    const absoluteTotals = analytics ? [
        { name: 'Aggregated Resource Inbound', Total: analytics.totalIncome, fill: '#10B981' },
        { name: 'Aggregated Resource Outbound', Total: analytics.totalExpenses, fill: '#F43F5E' }
    ] : [];

    const HEX_COLOR_INDEX_ARRAY = ['#6366F1', '#10B981', '#F59E0B', '#EF4444', '#8B5CF6', '#EC4899'];

    return (
        <div className="space-y-8">
            <div>
                <h1 className="text-2xl font-bold text-white">Advanced Analytical Control Dashboard</h1>
                <p className="text-slate-500 text-xs mt-0.5">Deep visualization processing layout across ledger indices</p>
            </div>

            <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
                <div className="glass-panel rounded-2xl p-6 shadow-xl">
                    <h3 className="text-xs font-mono uppercase tracking-wider text-slate-400 mb-6">Macro Resource Ratios Comparison</h3>
                    <div className="h-72">
                        <ResponsiveContainer width="100%" height="100%">
                            <BarChart data={absoluteTotals}>
                                <CartesianGrid strokeDasharray="3 3" stroke="#131B2E" vertical={false} />
                                <XAxis dataKey="name" stroke="#475569" fontSize={11} tickLine={false} />
                                <YAxis stroke="#475569" fontSize={11} tickLine={false} axisLine={false} />
                                <Tooltip contentStyle={{ backgroundColor: '#090D16', border: '1px solid #1E293B', borderRadius: '12px', fontSize: '11px' }} />
                                <Bar dataKey="Total" radius={[8, 8, 0, 0]} maxBarSize={60} />
                            </BarChart>
                        </ResponsiveContainer>
                    </div>
                </div>

                <div className="glass-panel rounded-2xl p-6 shadow-xl">
                    <h3 className="text-xs font-mono uppercase tracking-wider text-slate-400 mb-6">Structural Domain Segment Breakdown</h3>
                    <div className="h-72 flex items-center justify-center">
                        {distributionData.length > 0 ? (
                            <ResponsiveContainer width="100%" height="100%">
                                <PieChart>
                                    <Pie data={distributionData} dataKey="value" nameKey="name" cx="50%" cy="50%" outerRadius={80} innerRadius={55} paddingAngle={4}>
                                        {distributionData.map((entry, index) => (
                                            <Cell key={`cell-${index}`} fill={HEX_COLOR_INDEX_ARRAY[index % HEX_COLOR_INDEX_ARRAY.length]} />
                                        ))}
                                    </Pie>
                                    <Tooltip contentStyle={{ backgroundColor: '#090D16', border: '1px solid #1E293B', borderRadius: '12px', fontSize: '11px' }} />
                                    <Legend formatter={(value) => <span className="text-xs text-slate-400 font-mono">{value}</span>} />
                                </PieChart>
                            </ResponsiveContainer>
                        ) : (
                            <div className="text-xs font-mono text-slate-600">Dynamic array configuration registers clean empty values</div>
                        )}
                    </div>
                </div>
            </div>
        </div>
    );
};

export default Analytics;
```

### **👤 src/pages/Profile.tsx**

JavaScript

```
import React from 'react';
import { useAuth } from '../context/AuthContext';
import { Shield, HardDrive, Cpu } from 'lucide-react';

const Profile = () => {
    const { user } = useAuth();

    return (
        <div className="space-y-6 max-w-3xl">
            <div>
                <h1 className="text-2xl font-bold text-white">Active Operational Node Profile</h1>
                <p className="text-slate-500 text-xs mt-0.5">System infrastructure routing properties for authorized profile</p>
            </div>

            <div className="glass-panel rounded-2xl p-6 shadow-xl space-y-6">
                <div className="flex items-center gap-4 border-b border-slate-900 pb-5">
                    <div className="w-14 h-14 rounded-2xl bg-indigo-600/10 border border-indigo-500/20 flex items-center justify-center text-indigo-400 text-xl font-bold font-mono">
                        {user?.name?.charAt(0).toUpperCase()}
                    </div>
                    <div>
                        <h3 className="text-base font-semibold text-white">{user?.name}</h3>
                        <p className="text-xs font-mono text-slate-500 mt-0.5">Instance Client Platform Context Mapping</p>
                    </div>
                </div>

                <div className="grid grid-cols-1 md:grid-cols-2 gap-4 text-xs font-mono">
                    <div className="bg-slate-950/40 border border-slate-900 rounded-xl p-4 space-y-1">
                        <span className="text-slate-500 block">Ident String</span>
                        <span className="text-slate-200 text-sm font-sans">{user?.email}</span>
                    </div>
                    <div className="bg-slate-950/40 border border-slate-900 rounded-xl p-4 space-y-1">
                        <span className="text-slate-500 block">Security Hierarchy Access Level</span>
                        <span className="text-emerald-400 text-sm inline-flex items-center gap-1.5 font-sans font-medium">
                            <Shield className="h-3.5 w-3.5" /> Verified Standard Operator
                        </span>
                    </div>
                </div>

                <div className="border-t border-slate-900 pt-5 space-y-3">
                    <h4 className="text-xs font-mono uppercase tracking-wider text-slate-400">Environment Node Meta Matrices</h4>
                    <div className="space-y-2 text-xs text-slate-500 font-mono">
                        <div className="flex items-center gap-2"><Cpu className="h-3.5 w-3.5 text-slate-600" /> Virtual Engine Thread Context: Next-Gen React V18 client runtime</div>
                        <div className="flex items-center gap-2"><HardDrive className="h-3.5 w-3.5 text-slate-600" /> Layer Database Handshake Tunnel: Hibernate Custom JPA Transactional Proxy</div>
                    </div>
                </div>
            </div>
        </div>
    );
};

export default Profile;
```

### **❌ src/pages/NotFound.tsx**

JavaScript

```
import React from 'react';
import { Link } from 'react-router-dom';
import { Terminal } from 'lucide-react';

const NotFound = () => {
    return (
        <div className="min-h-screen flex items-center justify-center p-4">
            <div className="text-center space-y-4 max-w-md">
                <div className="inline-flex p-4 bg-rose-500/10 border border-rose-500/20 rounded-2xl text-rose-400 mb-2 animate-pulse">
                    <Terminal className="h-8 w-8" />
                </div>
                <h1 className="text-4xl font-extrabold text-white font-mono tracking-tight">404 // ROUTE_NOT_FOUND</h1>
                <p className="text-slate-500 text-xs font-mono leading-relaxed">
                    The entry path targeted by your navigation engine interface maps to an unallocated pointer address block within the platform router stack tree matrix.
                </p>
                <div className="pt-4">
                    <Link to="/dashboard" className="inline-block bg-slate-900 hover:bg-slate-800 border border-slate-800 hover:border-slate-700 text-slate-300 px-5 py-2 rounded-xl text-xs font-mono transition-all">
                        Return to Dashboard Terminal
                    </Link>
                </div>
            </div>
        </div>
    );
};

export default NotFound;
```

# **15\. Operational Micro-Services Deployment Engine**

### **📉 smart-expense-tracker-backend/Dockerfile**

Dockerfile

```
# Build Stage Target Configuration Environment
FROM maven:3.8.8-eclipse-temurin-17-alpine AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline -B
COPY src ./src
RUN mvn clean package -DskipTests

# High Efficiency Container Runtime Production Platform
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=builder /app/target/expense-tracker-1.0.0.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom", "-jar", "app.jar"]
```

### **📈 smart-expense-tracker-frontend/Dockerfile**

Dockerfile

```
# Compilation Infrastructure Layer Segment
FROM node:20-alpine AS compiler
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# High Performance Specialized Static File Server Distribution Framework
FROM nginx:alpine
COPY --from=compiler /app/dist /usr/share/nginx/html
# Custom Routing Configuration File for Structural Single Page Router Stability
RUN echo 'server { listen 80; location / { root /usr/share/nginx/html; index index.html; try_files $uri $uri/ /index.html; } }' > /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### **🐳 docker-compose.yml**

YAML

```
version: '3.8'

services:
  database_node:
    image: mysql:8.0
    container_name: mysql-production-terminal
    restart: always
    environment:
      MYSQL_DATABASE: expense_db
      MYSQL_ROOT_PASSWORD: password
    ports:
      - "3306:3306"
    volumes:
      - mysql_storage_cluster:/var/lib/mysql
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
      interval: 10s
      timeout: 5s
      retries: 5

  backend_node:
    build: ./smart-expense-tracker-backend
    container_name: springboot-api-engine
    restart: always
    ports:
      - "8080:8080"
    environment:
      DB_HOST: database_node
      DB_PORT: 3306
      DB_NAME: expense_db
      DB_USER: root
      DB_PASSWORD: password
    depends_on:
      database_node:
        condition: service_healthy

  frontend_node:
    build: ./smart-expense-tracker-frontend
    container_name: react-ui-client
    restart: always
    ports:
      - "80:80"
    depends_on:
      - backend_node

volumes:
  mysql_storage_cluster:
```

# **16\. Platform Operational Playbook Documentation**

This blueprint provides full infrastructure tracking configurations for development, orchestration staging, and secure cluster lifecycle execution.

## **⚙️ Project Ecosystem Overview**

The system architecture isolates database mutations within a dedicated MySQL volume container, utilizing structural entity relational models handled natively inside Spring Boot via an active Hibernate mapping proxy. The client layer establishes continuous authentication metrics across secure context vectors parsed through standard Axios transport intercepts.

## **🚀 Native Compilation & Runtime Instructions**

### **Local Backend Infrastructure Bootstrapping**

1. Verify target compilation prerequisites inside your environment terminal space: Java Development Kit 17 (JDK) and Maven.  
2. Initialize and configure standard local schema space within active local MySQL instances:  
3. SQL

```
CREATE DATABASE expense_db;
```

4.   
5. Route to the primary core API engine code configuration workspace block and deploy runtime context maps:  
6. Bash

```
cd smart-expense-tracker-backend
mvn spring-boot:run
```

7. 

### **Local Frontend Web App Frame Initiation**

1. Ensure the Node.js runtime environment (version 20 or higher) is configured on your execution machine.  
2. Navigate to the client UI application frame configuration tree and map dynamic packaging node profiles:  
3. Bash

```
cd smart-expense-tracker-frontend
npm install
```

4.   
5. Launch local runtime ecosystem instance engine metrics tracking loop:  
6. Bash

```
npm run dev
```

7. 

## **🐳 Orchestrated Multi-Container Automated Deployment**

To spin up the multi-tier production-ready environment, execute the orchestrator command loop directly from the project root configuration mapping space:

Bash

```
docker-compose up --build -d
```

The architecture handles microservice startup dependencies asynchronously through built-in database check metrics, exposing the application layout on your network topology interfaces:

* **Premium Client Application Space UI Framework Dashboard Engine Mapping Access:** http://localhost:80  
* **Centralized Secure REST Controller API Root Context Gateway Entry Access:** http://localhost:8080

**RLHF Evaluation & Scoring**

### **1 \- Correctness — 3.5/5**

For Correctness, I would rate the response around 3.5/5 because the overall architecture and code structure were technically good, and the response included working concepts like JWT authentication, CRUD APIs, React integration, and MySQL connectivity. However, several parts still felt unrefined and would likely require debugging and cleanup before real production use. Some generated labels and API messages were unrealistic and overly AI-generated, which reduced the overall accuracy and professionalism.

**2 \- Relevance – 4.7/5**

The reason I like it is that it follows the instructions closely. It stays focused on the Smart Expense Tracker project all the time. The Smart Expense Tracker project has everything we asked for like React, Tailwind CSS, Spring Boot, JWT authentication, MySQL and Docker. It also has a dashboard with analytics a design that works well on all devices and a good architecture with parts. The Smart Expense Tracker project doesn't go off track. It doesn't talk about things that're not related to the Smart Expense Tracker project. This shows that the Smart Expense Tracker project is very good at following instructions. It's also very relevant, to what we're trying to do with the Smart Expense Tracker project.

**3 \-** **Completeness — 4.2/5**

For Completeness, I would rate the response around 4.2/5 because it covered a large portion of the requested application, including frontend pages, backend APIs, authentication, dashboard features, reusable components, and deployment-related concepts. The project structure was extensive and detailed. However, some advanced features like full production deployment setup, pagination, toast notifications, and deeper analytics functionality were either partially implemented or missing.

**4 \-** **Style & Presentation — 3.8/5**

**For Style & Presentation, I would rate the response around 3.8/5 because the formatting, structure, and organization were visually clean and professional. The UI styling direction also looked modern and premium. However, many labels and texts were overly futuristic and robotic, which reduced realism and made the application feel less like a practical fintech product and more like a sci-fi dashboard.**

**5 \- Coherence – 3.9/5**

I think the Coherence of the response is around 3.9 out of 5\. The frontend and backend of the system were connected well and the application flow made sense most of the time.. The problem is that the words and style used to describe the product kept changing. Sometimes it sounded like a fintech product sometimes it sounded like it was about infrastructure. Sometimes it sounded like system monitoring. This made the product seem an inconsistent. The Coherence of the response was not perfect because the terminology and branding style were not the same all the time. The Coherence is important for the product identity and, in this case the product identity was not very clear because of the changing terminology and branding style of the fintech product and the infrastructure and system monitoring parts.

**6 \-** **Helpfulness — 4.3/5**

For Helpfulness, I would rate the response around 4.3/5 because it provides a strong starting point for developers building a full-stack expense tracker. The generated architecture, reusable components, API setup, and authentication flow are useful for learning and development purposes. Although refinement is still needed before deployment, the response significantly reduces development effort and offers a valuable project foundation.

**7 \- Creativity – 4.5/5**

The UI design direction, dashboard styling and overall fintech SaaS look were really impressive and more creative, than AI-generated outputs. The use of glassmorphism modern layouts and premium dashboard ideas made the project stand out. However sometimes the creativity went overboard with naming conventions and terminology which made it harder to use and less realistic. The project still had a lot of elements and the Creativity was still high. The UI design and dashboard styling showcased Creativity.

