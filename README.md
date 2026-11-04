

LOGIN.HTML

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

<form action="servlet1" method="post">

	<input type ="text" name="uname" placeholder="enter username" > <br/> <br/>
	<input type ="text" name="upassword" placeholder="enter userpassword" >
	<input type="submit" value="Login">

</form>

</body>
</html>

***************************************************************

Login.servlet1

in javapackeg name == com.js
AND FILE NAME == SERVLET1


protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		String uname = request.getParameter("uname");
		String upassword = request.getParameter("upassword");
		
		try {
			Class.forName("com.mysql.cj.jdbc.Driver");
			
			Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/exam", "root", "parth@98796");
			
			PreparedStatement ps = con.prepareStatement("select * from user where uname = ? and upassword = ?");
			ps.setString(1, uname);
			ps.setString(2, upassword);
			
			ResultSet rs = ps.executeQuery();	
			
			if(rs.next())
			{
				response.sendRedirect("Home.html");
			}
			else
			{
				response.sendRedirect("error.html");
			}
			rs.close();
			con.close();
			ps.close();	
		    }
		catch (Exception e){
			e.printStackTrace();
		}
	}

}


***************************************************************


Display.jsp 

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.sql.*" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

<table>

<tr>
<th>Id</th>
<th>Name</th>
<th>Gender</th>
<th>City</th>
</tr>

<%

try{
	Class.forName("com.mysql.cj.jdbc.Driver");
	
	Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/exam", "root", "parth@98796");
	
	Statement stmt = con.createStatement();
	ResultSet rs = stmt.executeQuery("Select * from student");
	
	while(rs.next()){
%>
		<tr>
		<td> <%=  rs.getInt("id")%> </td>
		<td> <%=  rs.getString("name")%> </td>
		<td> <%=  rs.getString("gender")%> </td>
		<td> <%=  rs.getString("city")%> </td>
		</tr>

<% 
	}
	con.close();
}
catch(Exception e){
	out.println("Error" + e.getMessage());
}

%>

</table>

</body>
</html>
