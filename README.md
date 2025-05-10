# JSP_server_programing

# 1주차: 웹 백엔드 개요 및 개발 환경 세팅

## 학습 목표
- 웹 백엔드의 기본 개념을 이해한다.
- JSP/Servlet이 동작하는 구조를 파악한다.
- 개발 환경을 세팅하고 테스트 페이지를 출력해본다.

---

## 학습 내용

### 1. 웹의 기본 동작 구조
- 클라이언트(브라우저)가 서버에 요청(request)을 보냄
- 서버는 해당 요청을 처리하고, HTML 형태로 응답(response)을 반환함
- 백엔드는 이 “요청 → 처리 → 응답” 과정에서 핵심적인 역할을 함

### 2. JSP & Servlet의 역할
- Servlet: Java 기반의 서버 측 로직을 담당. 요청을 받고 데이터를 처리함
- JSP (Java Server Pages): HTML 안에 Java 코드를 넣어 동적인 웹페이지를 생성

### 3. 개발 환경 세팅
- IDE: Eclipse IDE for Enterprise Java
- 서버: Apache Tomcat 9.x 이상
- DBMS: MySQL 8.x
- JDBC Connector: MySQL Connector/J

### 4. 폴더 구조 및 첫 테스트
- JSP 파일을 `WebContent` 또는 `src/main/webapp` 폴더에 위치시킴
- 기본 JSP 파일 생성:

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"%>
<html>
<head><title>첫 JSP</title></head>
<body>
  <h1>JSP 첫 화면입니다!</h1>
  <%
    out.println("현재 시간: " + new java.util.Date());
  %>
</body>
</html>

# 2주차: JSP 문법 기초

## 학습 목표
- JSP 기본 문법을 이해하고 활용할 수 있다.
- JSP 내장 객체를 이용해 동적인 페이지를 구현한다.
- 간단한 연산, 조건문, 반복문 등을 JSP에서 적용해본다.

---

## 학습 내용

### 1. JSP의 기본 구조
- JSP 파일은 HTML 문서 안에 Java 코드를 삽입할 수 있음
- JSP 구문 형태:
  - `<% ... %>`: Java 코드 (스크립틀릿)
  - `<%= ... %>`: 출력 표현식
  - `<%@ ... %>`: 디렉티브 (설정용)

### 2. JSP 내장 객체
- JSP에서 기본으로 제공하는 객체들 (Servlet에서 자동 생성됨)
  - `request`, `response`, `session`, `application`, `out`, `config`, `pageContext`, `exception`, `page`

### 3. JSP에서 사용하는 Java 문법
- 변수 선언, 연산자 사용
- 조건문: `if`, `if-else`
- 반복문: `for`, `while`
- 예시:

```jsp
<%
  int number = 5;
  if (number % 2 == 0) {
%>
    <p><%= number %>는 짝수입니다.</p>
<%
  } else {
%>
    <p><%= number %>는 홀수입니다.</p>
<%
  }
%>

# 3주차: MySQL과 JDBC 연결

## 학습 목표
- MySQL 기본 사용법을 익힌다.
- JDBC를 통해 JSP와 MySQL을 연동한다.
- DB 연결 및 데이터 조회 테스트를 수행한다.

---

## 학습 내용

### 1. MySQL 기본 개념 및 설치 확인
- DBMS: 데이터를 구조적으로 저장하고 관리하는 시스템
- MySQL은 오픈소스 RDBMS로 JSP/Servlet과 자주 사용됨
- 확인 사항:
  - MySQL 설치 및 root 계정 접속
  - 테스트용 데이터베이스 및 테이블 생성

### 2. JDBC(Java Database Connectivity)
- Java에서 데이터베이스에 접근하기 위한 표준 API
- 주요 구성 요소:
  - DriverManager
  - Connection
  - Statement / PreparedStatement
  - ResultSet

<%@ page import="java.sql.*" %>
<%
  String url = "jdbc:mysql://localhost:3306/testdb?useSSL=false&serverTimezone=UTC";
  String user = "root";
  String password = "비밀번호";
  
  Class.forName("com.mysql.cj.jdbc.Driver");
  Connection conn = DriverManager.getConnection(url, user, password);
  Statement stmt = conn.createStatement();
  ResultSet rs = stmt.executeQuery("SELECT * FROM users");

  while (rs.next()) {
    out.println("<p>이름: " + rs.getString("name") + "</p>");
  }

  rs.close();
  stmt.close();
  conn.close();
%>
