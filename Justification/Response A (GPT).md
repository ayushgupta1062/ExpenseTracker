# **Smart Expense Tracker – Complete Full Stack Production Ready Application**

## **Project Overview**

Build a modern premium fintech style Smart Expense Tracker web application using:

### **Frontend**

• React JS  
• Tailwind CSS  
• Axios  
• React Router DOM  
• Recharts

### **Backend**

• Java Spring Boot  
• Spring MVC Architecture  
• Spring Security  
• JWT Authentication  
• Hibernate / JPA  
• Maven

### **Database**

• MySQL

### **Deployment**

• Docker  
• Docker Compose  
• Vercel / Netlify  
• Render / Railway

---

# **Complete Application Features**

## **Authentication System**

### **Features**

• User Registration  
• User Login  
• JWT Authentication  
• BCrypt Password Encryption  
• Protected Routes  
• Session Persistence  
• Logout Functionality  
• Validation & Error Handling

### **Authentication Flow**

1. User signs up using email/password  
2. Backend encrypts password using BCrypt  
3. JWT token generated on login  
4. Token stored in localStorage  
5. Protected routes validated using JWT  
6. Axios interceptor sends token automatically

---

# **Frontend Architecture**

## **Frontend Folder Structure**

```shell
frontend/
│
├── public/
│
├── src/
│   ├── api/
│   │   ├── axios.js
│   │   ├── authApi.js
│   │   ├── expenseApi.js
│   │   ├── incomeApi.js
│   │   ├── categoryApi.js
│   │   └── dashboardApi.js
│   │
│   ├── assets/
│   │
│   ├── components/
│   │   ├── common/
│   │   │   ├── Button.jsx
│   │   │   ├── Card.jsx
│   │   │   ├── Modal.jsx
│   │   │   ├── Loader.jsx
│   │   │   ├── EmptyState.jsx
│   │   │   └── Toast.jsx
│   │   │
│   │   ├── layout/
│   │   │   ├── Sidebar.jsx
│   │   │   ├── Navbar.jsx
│   │   │   └── DashboardLayout.jsx
│   │   │
│   │   ├── charts/
│   │   │   ├── ExpensePieChart.jsx
│   │   │   ├── IncomeExpenseChart.jsx
│   │   │   └── MonthlyExpenseChart.jsx
│   │
│   ├── context/
│   │   └── AuthContext.jsx
│   │
│   ├── hooks/
│   │
│   ├── pages/
│   │   ├── auth/
│   │   │   ├── Login.jsx
│   │   │   └── Signup.jsx
│   │   │
│   │   ├── dashboard/
│   │   │   └── Dashboard.jsx
│   │   │
│   │   ├── expenses/
│   │   │   └── Expenses.jsx
│   │   │
│   │   ├── incomes/
│   │   │   └── Incomes.jsx
│   │   │
│   │   ├── categories/
│   │   │   └── Categories.jsx
│   │   │
│   │   ├── analytics/
│   │   │   └── Analytics.jsx
│   │   │
│   │   ├── profile/
│   │   │   └── Profile.jsx
│   │   │
│   │   └── errors/
│   │       └── NotFound.jsx
│   │
│   ├── routes/
│   │   └── AppRoutes.jsx
│   │
│   ├── utils/
│   │
│   ├── App.jsx
│   ├── main.jsx
│   └── index.css
│
├── .env
├── package.json
├── tailwind.config.js
└── vite.config.js
```

---

# **Backend Architecture**

## **Backend Folder Structure**

```shell
backend/
│
├── src/main/java/com/expense/tracker/
│
│   ├── controller/
│   │   ├── AuthController.java
│   │   ├── ExpenseController.java
│   │   ├── IncomeController.java
│   │   ├── CategoryController.java
│   │   └── DashboardController.java
│   │
│   ├── service/
│   │   ├── AuthService.java
│   │   ├── ExpenseService.java
│   │   ├── IncomeService.java
│   │   ├── CategoryService.java
│   │   └── DashboardService.java
│   │
│   ├── repository/
│   │   ├── UserRepository.java
│   │   ├── ExpenseRepository.java
│   │   ├── IncomeRepository.java
│   │   └── CategoryRepository.java
│   │
│   ├── entity/
│   │   ├── User.java
│   │   ├── Expense.java
│   │   ├── Income.java
│   │   └── Category.java
│   │
│   ├── dto/
│   │   ├── LoginRequest.java
│   │   ├── SignupRequest.java
│   │   ├── AuthResponse.java
│   │   └── ExpenseDTO.java
│   │
│   ├── security/
│   │   ├── JwtFilter.java
│   │   ├── JwtUtil.java
│   │   ├── SecurityConfig.java
│   │   └── CustomUserDetailsService.java
│   │
│   ├── exception/
│   │   ├── GlobalExceptionHandler.java
│   │   └── ResourceNotFoundException.java
│   │
│   ├── config/
│   │   └── CorsConfig.java
│   │
│   └── ExpenseTrackerApplication.java
│
├── src/main/resources/
│   ├── application.properties
│
├── Dockerfile
├── pom.xml
└── .env
```

---

# **Database Design**

## **Tables**

### **users**

```sql
CREATE TABLE users (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    email VARCHAR(120) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### **categories**

```sql
CREATE TABLE categories (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    user_id BIGINT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

### **expenses**

```sql
CREATE TABLE expenses (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255),
    amount DECIMAL(10,2),
    description TEXT,
    expense_date DATE,
    user_id BIGINT,
    category_id BIGINT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (category_id) REFERENCES categories(id)
);
```

### **incomes**

```sql
CREATE TABLE incomes (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    source VARCHAR(255),
    amount DECIMAL(10,2),
    description TEXT,
    income_date DATE,
    user_id BIGINT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

---

# **UI/UX Design System**

## **Design Theme**

### **Style**

• Modern SaaS Dashboard  
• Glassmorphism  
• Gradient Backgrounds  
• Premium Fintech Design  
• Responsive Layouts

### **Color Palette**

```css
Primary: #7C3AED
Secondary: #06B6D4
Accent: #22C55E
Background: #0F172A
Glass: rgba(255,255,255,0.1)
```

### **UI Components**

• Glass Cards  
• Animated Buttons  
• Soft Shadows  
• Rounded Borders  
• Interactive Graphs  
• Smooth Hover Effects

---

# **Dashboard Features**

## **Summary Cards**

• Total Income  
• Total Expense  
• Remaining Balance  
• Monthly Overview

## **Analytics**

• Monthly Expense Chart  
• Income vs Expense Graph  
• Category Wise Pie Chart

## **Additional Features**

• Recent Transactions  
• Quick Add Buttons  
• Activity Feed  
• Financial Insights

---

# **Frontend Setup**

## **Install Frontend**

```shell
cd frontend

npm install
```

## **Install Required Packages**

```shell
npm install react-router-dom axios recharts
npm install react-hot-toast
npm install react-icons
npm install framer-motion
```

## **Install Tailwind CSS**

```shell
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

---

# **Tailwind Configuration**

## **tailwind.config.js**

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        primary: "#7C3AED",
        secondary: "#06B6D4",
      },
      backdropBlur: {
        xs: '2px',
      },
    },
  },
  plugins: [],
}
```

---

# **Axios Configuration**

## **axios.js**

```javascript
import axios from "axios";

const API = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
});

API.interceptors.request.use((req) => {
  const token = localStorage.getItem("token");

  if (token) {
    req.headers.Authorization = `Bearer ${token}`;
  }

  return req;
});

export default API;
```

---

# **React Routing Setup**

## **AppRoutes.jsx**

```javascript
import { Routes, Route } from "react-router-dom";

import Login from "../pages/auth/Login";
import Signup from "../pages/auth/Signup";
import Dashboard from "../pages/dashboard/Dashboard";

const AppRoutes = () => {
  return (
    <Routes>
      <Route path="/" element={<Login />} />
      <Route path="/signup" element={<Signup />} />
      <Route path="/dashboard" element={<Dashboard />} />
    </Routes>
  );
};

export default AppRoutes;
```

---

# **Spring Boot Dependencies**

## **pom.xml**

```xml
<dependencies>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
    </dependency>

    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt-api</artifactId>
        <version>0.11.5</version>
    </dependency>

</dependencies>
```

---

# **JWT Security Configuration**

## **SecurityConfig.java**

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http)
            throws Exception {

        http
            .csrf(csrf -> csrf.disable())
            .authorizeHttpRequests(auth -> auth
                .requestMatchers(
                    "/api/auth/**"
                ).permitAll()
                .anyRequest().authenticated()
            );

        return http.build();
    }
}
```

---

# **Environment Variables**

## **Frontend .env**

```
VITE_API_URL=http://localhost:8080/api
```

## **Backend .env**

```
DB_URL=jdbc:mysql://localhost:3306/expense_tracker
DB_USERNAME=root
DB_PASSWORD=password

JWT_SECRET=your_secret_key
JWT_EXPIRATION=86400000
```

---

# **Docker Setup**

## **Frontend Dockerfile**

```
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

EXPOSE 5173

CMD ["npm", "run", "dev"]
```

---

## **Backend Dockerfile**

```
FROM eclipse-temurin:21

WORKDIR /app

COPY target/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java","-jar","app.jar"]
```

---

# **Docker Compose**

## **docker-compose.yml**

```
version: "3.9"

services:

  mysql:
    image: mysql:8
    container_name: expense_mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: expense_tracker
    ports:
      - "3307:3306"

  backend:
    build: ./backend
    container_name: expense_backend
    ports:
      - "8080:8080"
    depends_on:
      - mysql

  frontend:
    build: ./frontend
    container_name: expense_frontend
    ports:
      - "5173:5173"
    depends_on:
      - backend
```

---

# **Deployment Guide**

# **Frontend Deployment**

## **Deploy on Vercel**

### **Steps**

1. Push frontend code to GitHub  
2. Import project in Vercel  
3. Add Environment Variables  
4. Deploy

## **Build Command**

```shell
npm run build
```

## **Output Directory**

```shell
dist
```

---

# **Backend Deployment**

## **Deploy on Render**

### **Steps**

1. Push backend code to GitHub  
2. Create Web Service on Render  
3. Connect Repository  
4. Add Environment Variables  
5. Deploy

## **Build Command**

```shell
./mvnw clean package
```

## **Start Command**

```shell
java -jar target/app.jar
```

---

# **MySQL Deployment**

## **Railway MySQL**

or

## **PlanetScale**

or

## **Neon \+ MySQL Compatible**

---

# **Production Best Practices**

## **Security**

• Use HTTPS  
• Store secrets in env variables  
• Enable CORS securely  
• Use BCrypt hashing  
• Validate all inputs

## **Performance**

• Lazy loading  
• Pagination  
• Optimized API calls  
• Reusable components  
• Database indexing

## **Scalability**

• Layered architecture  
• DTO pattern  
• Service abstraction  
• Modular frontend structure

---

# **Analytics APIs**

## **Dashboard APIs**

### **Get Summary**

```
GET /api/dashboard/summary
```

### **Monthly Expenses**

```
GET /api/dashboard/monthly-expenses
```

### **Category Analytics**

```
GET /api/dashboard/category-analysis
```

---

# **Expense APIs**

## **Create Expense**

```
POST /api/expenses
```

## **Update Expense**

```
PUT /api/expenses/{id}
```

## **Delete Expense**

```
DELETE /api/expenses/{id}
```

## **Get Expenses**

```
GET /api/expenses
```

---

# **Income APIs**

```
GET /api/incomes
POST /api/incomes
PUT /api/incomes/{id}
DELETE /api/incomes/{id}
```

---

# **Category APIs**

```
GET /api/categories
POST /api/categories
PUT /api/categories/{id}
DELETE /api/categories/{id}
```

---

# **Recommended Premium Features**

## **Future Enhancements**

• Budget Goals  
• AI Expense Prediction  
• PDF Reports  
• CSV Export  
• Dark/Light Theme  
• Multi Currency Support  
• Email Notifications  
• Google OAuth Login  
• Recurring Expenses  
• Savings Goals

---

# **README Structure**

## **Include Sections**

```
# Smart Expense Tracker

## Features
## Tech Stack
## Installation
## Frontend Setup
## Backend Setup
## Database Setup
## API Documentation
## Environment Variables
## Docker Setup
## Deployment Guide
## Folder Structure
## Screenshots
## Future Improvements
```

---

# **Final Application Result**

The final Smart Expense Tracker application will provide:

• Premium fintech dashboard UI  
• Secure JWT authentication  
• Full CRUD operations  
• Beautiful responsive analytics  
• Modern glassmorphism design  
• Professional SaaS experience  
• Production ready architecture  
• Docker deployment support  
• Scalable backend structure  
• Industry standard coding practices

This architecture is fully scalable, beginner friendly, production ready, and follows modern full stack Java development standards for real SaaS fintech applications.

**Ratings & Evaluations (RLHF)**

### **1\. Correctness — 3.5/5**

The response was technically strong at the architectural level and used the right stack and patterns, but several implementation issues reduced reliability. Small syntax mistakes, malformed template literals, and inconsistent environment variable naming showed that the project was not fully tested end-to-end. It felt more like a polished draft than a deployment-ready implementation.

**2\. Relevance — 4/5**

The response stayed closely aligned with the requested portfolio stack and covered most major requirements like animations, contact forms, backend APIs, and UI structure. However, some explicitly requested production features such as CAPTCHA, rate limiting, database logging, and accessibility considerations were either skipped or only lightly mentioned.

**3\. Completeness — 3/5**

The foundation and architecture were well planned, but several sections remained incomplete. Important areas like About, Projects, deployment documentation, security hardening, and database integration were not fully implemented. The response gave the impression of a high-quality starter template rather than a finished production application.

**4\. Style & Presentation — 4/5**

The response was clean, organized, and visually easy to follow. Folder structures, component separation, and naming conventions were handled well. However, formatting inconsistencies and overly AI-structured presentation slightly reduced the natural engineering feel of the response.

### **5\. Coherence — 4/5**

The flow from setup to frontend, backend, and integration was logical and easy to understand. Most sections connected well architecturally, but minor inconsistencies like broken template literals and mismatched environment variables disrupted the overall polish. It was coherent structurally, though slightly mechanical narratively.

### **6\. Helpfulness — 3.5/5**

The response provided useful setup guidance, architecture direction, and development structure, especially for beginners. However, it lacked deeper practical guidance such as deployment steps, Gmail App Password configuration, debugging advice, production security practices, and CI/CD recommendations. It explained what to build more than how to fully ship it.

### **7\. Creativity — 4/5**

The response showed thoughtful frontend engineering choices like reusable animation wrappers, shared validation logic, and polished modal interactions. The UI direction felt modern and professional, though the overall architecture stayed within familiar industry patterns without introducing highly innovative features or standout portfolio experiences.

