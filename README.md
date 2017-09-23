# servlet
package com.davd.dao;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;

import com.davd.Bean.UserBean;

public class UserDao {
	
	public List<UserBean> select(String name)
	{
		try {
			Class.forName("com.mysql.jdbc.Driver");
			
			String sql="select * from User where name='"+name+"'";
			System.out.println(sql);
			Connection conn;
			try {
				conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/web?useUnicode=true&characterEncoding=utf-8&useSSL=false","root","root");
				Statement stat=conn.createStatement();
				ResultSet rs = stat.executeQuery(sql);
				ArrayList<UserBean> al = new ArrayList<UserBean>(); 
				while(rs.next())
				{
					UserBean ub = new UserBean();
					ub.setName(rs.getString(2));
					ub.setPassword(rs.getString(3));
					al.add(ub);
				}
				return al;	
				
				
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			
		} catch (ClassNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return null;
	}
	//注册用户方法
	public void insertUser(UserBean ub){
		try {
			Class.forName("com.mysql.jdbc.Driver");
			
			String sql="INSERT INTO `user` VALUES ("+0+",'"+ub.getName()+"','"+ub.getPassword()+"');";
			System.out.println(sql);
			Connection conn;
			try {
				conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/web?useUnicode=true&characterEncoding=utf-8&useSSL=false","root","root");
				Statement stat=conn.createStatement();
				stat.execute(sql);
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			
		} catch (ClassNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
			
	}
	
	
	
}

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.davd.Bean.UserBean;
import com.davd.dao.UserDao;

public class LoginServlet extends HttpServlet{
	
	
	public void init()
	{
		
	}
	public void destroy()
	{
		
	}
	public void doPost(HttpServletRequest request,HttpServletResponse response)
	{
		//获取请求参数
		String name =request.getParameter("username");
		String password = request.getParameter("password");
		
		//实例化一个dao类，dao类负操作数据库插入
		UserDao ud = new UserDao();
		List<UserBean>  ab =new ArrayList<UserBean>();
		ab = ud.select(name);
		if(ab.size()>0)
		{
			try {
				response.sendRedirect("index.html");
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}else{
			try {
				response.sendRedirect("unregister.html");
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			
		}
		
	}
}
