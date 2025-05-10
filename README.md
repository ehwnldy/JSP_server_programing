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

# 4주차: Create 기능 구현 (Insert)

## 학습 목표
- 사용자 입력을 받아 MySQL에 데이터를 추가하는 기능을 구현한다.
- JSP에서 HTML 폼을 작성하고, 이를 통해 데이터를 서버로 전송한다.
- JDBC를 이용하여 INSERT 쿼리를 실행하고, 데이터를 DB에 저장한다.

---

## 학습 내용

### 1. HTML 폼을 통한 사용자 입력
- 사용자로부터 데이터를 입력받기 위해 HTML 폼을 작성함
- 폼 태그 사용 예시:
```html
<form action="insert.jsp" method="post">
  <label for="title">제목:</label>
  <input type="text" id="title" name="title"><br><br>
  
  <label for="content">내용:</label>
  <textarea id="content" name="content"></textarea><br><br>
  
  <input type="submit" value="등록">
</form>

<%@ page import="java.sql.*" %>
<%
  String title = request.getParameter("title");
  String content = request.getParameter("content");
  
  String url = "jdbc:mysql://localhost:3306/testdb?useSSL=false&serverTimezone=UTC";
  String user = "root";
  String password = "비밀번호";
  
  Class.forName("com.mysql.cj.jdbc.Driver");
  Connection conn = DriverManager.getConnection(url, user, password);
  
  String query = "INSERT INTO board (title, content) VALUES (?, ?)";
  PreparedStatement pstmt = conn.prepareStatement(query);
  pstmt.setString(1, title);
  pstmt.setString(2, content);
  
  int result = pstmt.executeUpdate();
  if(result > 0) {
      out.println("게시글이 성공적으로 등록되었습니다.");
  } else {
      out.println("게시글 등록에 실패했습니다.");
  }

  pstmt.close();
  conn.close();
%>


# 5주차: Read 기능 구현 (게시글 목록)

## 학습 목표
- MySQL에서 데이터를 조회하여 게시글 목록을 출력하는 기능을 구현한다.
- JDBC를 이용해 SELECT 쿼리를 실행하고, 결과를 JSP에서 동적으로 출력한다.
- 게시글 목록을 페이지에 표시하기 위한 기본적인 디자인과 동작을 구현한다.

<%@ page import="java.sql.*" %>
<%
  String url = "jdbc:mysql://localhost:3306/testdb?useSSL=false&serverTimezone=UTC";
  String user = "root";
  String password = "비밀번호";
  
  Class.forName("com.mysql.cj.jdbc.Driver");
  Connection conn = DriverManager.getConnection(url, user, password);
  
  String query = "SELECT * FROM board ORDER BY id DESC";
  Statement stmt = conn.createStatement();
  ResultSet rs = stmt.executeQuery(query);

  while (rs.next()) {
    int id = rs.getInt("id");
    String title = rs.getString("title");
    String content = rs.getString("content");
%>
    <div>
      <h3><%= title %></h3>
      <p><%= content %></p>
      <a href="view.jsp?id=<%= id %>">상세보기</a>
    </div>
<%
  }

  rs.close();
  stmt.close();
  conn.close();
%>

# 6주차: Read 기능 구현 (게시글 상세보기)

## 학습 목표
- 게시글 목록에서 선택한 게시글을 상세보기 페이지에 출력하는 기능을 구현한다.
- `id` 값을 URL로 전달하여 특정 게시글을 조회한다.
- 게시글의 세부 정보를 페이지에 동적으로 출력한다.

<%@ page import="java.sql.*" %>
<%
  String id = request.getParameter("id");
  String url = "jdbc:mysql://localhost:3306/testdb?useSSL=false&serverTimezone=UTC";
  String user = "root";
  String password = "비밀번호";
  
  Class.forName("com.mysql.cj.jdbc.Driver");
  Connection conn = DriverManager.getConnection(url, user, password);
  
  String query = "SELECT * FROM board WHERE id = ?";
  PreparedStatement pstmt = conn.prepareStatement(query);
  pstmt.setInt(1, Integer.parseInt(id));
  ResultSet rs = pstmt.executeQuery();
  
  if (rs.next()) {
    String title = rs.getString("title");
    String content = rs.getString("content");
    String createdAt = rs.getString("created_at");
%>
  <h2><%= title %></h2>
  <p><%= content %></p>
  <p><%= createdAt %></p>
<%
  } else {
    out.println("해당 게시글을 찾을 수 없습니다.");
  }

  rs.close();
  pstmt.close();
  conn.close();
%>
