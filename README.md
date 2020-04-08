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
## 시험sql
```
create table member_tbl_02(
custno NUMBER(6) PRIMARY key,
custname VARCHAR2(20),
phone varchar2(13),
address VARCHAR2(60),
joindate date,
grade CHAR(1),
city CHAR(2)
);

create table money_tbl_02(
custno NUMBER(6) ,
salenol NUMBER(8),
pcost NUMBER(8),
amount NUMBER(4),
price NUMBER(8),
pcode VARCHAR2(4),
sdate date,
PRIMARY key(custno,salenol)
);
insert into money_tbl_02 values(100001,20160001,500,5,2500,'a001',date'2016-01-01');
insert into money_tbl_02 values(100001,20160002,1000,4,4000,'a002',date'2016-01-01');
insert into money_tbl_02 values(100001,20160003,500,3,1500,'a008',date'2016-01-01');
insert into money_tbl_02 values(100002,20160004,2000,1,2000,'a004',date'2016-01-02');
insert into money_tbl_02 values(100002,20160005,500,1,500,'a001',date'2016-01-03');
insert into money_tbl_02 values(100003,20160006,1500,2,3000,'a003',date'2016-01-03');
insert into money_tbl_02 values(100004,20160007,500,2,1000,'a001',date'2016-01-04');
insert into money_tbl_02 values(100004,20160008,300,1,300,'a005',date'2016-01-04');
insert into money_tbl_02 values(100004,20160009,600,1,600,'a006',date'2016-01-04');
insert into money_tbl_02 values(100004,20160010,3000,1,3000,'a007',date'2016-01-06');
commit;

select * from member_tbl_02;
select * from money_tbl_02;

select r.custno,r.custname,r.grade,sum(y.price) as sales from member_tbl_02 r join money_tbl_02 y on(r.custno=y.custno) group by r.custno, r.custname, r.grade order by sales desc;
```
