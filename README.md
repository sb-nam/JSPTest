# JSPTest
## jsp 회원 가입 페이지 
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>

</head>
<body>
	<form name="joinform" action="joinProcess.jsp" method="post">
		<table border=1>
			<tr>
				<td colspan="2" class="td_title">회원 가입 페이지</td>
			</tr>
			<tr>
				<td><label for="id">아이디 : </label></td>
				<td><input type="text" name="id" id="id" required="required"/></td>
			</tr>
			<tr>
				<td><label for="pass">비밀번호 :</label></td>
				<td><input type="password" name="pass" id="pass" required="required"/></td>
			</tr>
			<tr>
				<td><label for="name">이름 :</label></td>
				<td><input type="text" name="name" id="name" required="required"/></td>
			</tr>
			<tr>
				<td><label for="age">나이 :</label></td>
				<td><input type="number" name="age" id="age" required="required"/></td>
			</tr>
			<tr>
				<td><label for="gender">성별 :</label></td>
				<td><input type="radio" name="gender" id="gender" value="남"
					checked />남자 <input type="radio" name="gender" id="gender"
					value="여" />여자</td>
			</tr>
			<tr>
				<td><label for="email">이메일 주소 :</label></td>
				<td><input type="text" name="email" id="email" /></td>
			</tr>

			<tr>
				<td colspan="2">
				<input type="submit" value="회원 가입">
				<input type="reset" value="다시 입력">
			</tr>
		</table>
	</form>

</body>

</html>
```
## jsp 아이디 중복 확인
```
<%@page import="javax.sql.DataSource"%>
<%@page import="javax.naming.*"%>
<%@page import="java.sql.*"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
   request.setCharacterEncoding("UTF-8");
   String id=request.getParameter("id");
   String pass=request.getParameter("pass");
   String name=request.getParameter("name");
   int age = Integer.parseInt(request.getParameter("age"));
   String gender = request.getParameter("gender");
   String email = request.getParameter("email");
   
   Connection conn =null;
   PreparedStatement pstmt = null;
   try{
      Context init = new InitialContext();
      DataSource ds= (DataSource) init.lookup("java:comp/env/jdbc/OracleDB");
      conn = ds.getConnection();
      
      pstmt = conn.prepareStatement("INSERT INTO member VALUES(?,?,?,?,?,?)");
      pstmt.setString(1,id);
      pstmt.setString(2,pass);
      pstmt.setString(3,name);
      pstmt.setInt(4,age);
      pstmt.setString(5,gender);
      pstmt.setString(6,email);
      int result=pstmt.executeUpdate();
      
      if(result !=0){
         out.println("<script>");
         out.println("alert('입력완료!')");
         out.println("location.href='joinForm.jsp'");
         out.println("</script>");
      }
      else{
         out.println("<script>");
         out.println("location.href='joinForm.jsp'");
         out.println("</script>");
      }
      
      
   }
   catch(SQLIntegrityConstraintViolationException e){
     
      out.println("<script>");
      out.println("alert('중복된 ID입니다.')");
      out.println("location.href='joinForm.jsp'");
      out.println("</script>");
   }
   catch(Exception e){
      e.printStackTrace();
   }finally{
      try{
         pstmt.close();
         conn.close();
      }catch(Exception e){
         e.printStackTrace();
      }
   }
%>
```
