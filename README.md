# JdbcSql_Dynamic-Program

import java.sql.Connection;
import java.sql.Date;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Scanner;

public class store 
{
	Scanner sc=new Scanner(System.in);
	int p_id;
	String p_name;
	int p_price;
	int p_quantity;

	public void operations() throws SQLException 
	{
		try
		{
			Class.forName("com.mysql.cj.jdbc.Driver");
			Connection conn;
			conn=DriverManager.getConnection("jdbc:mysql://localhost:3306/shop","root","root");
			while(true)
			{
			System.out.println("Enter your choice from below options");
			System.out.println("1:for showing the product with there quantity");
			System.out.println("2:for add the new product in the System");
			System.out.println("3:for Updating the quantity & price in existing product");
			System.out.println("4:for showing product according to the product id");
			System.out.println("5:for deleting the product from the system");
			System.out.println("6:for showing customise list");
			System.out.println("7:Exit");
			int ch=sc.nextInt();
			switch (ch) 
			{
			case 1:
				view(conn);
				break;
			case 2:
				new_input(conn);
				break;
			case 3:
				update(conn);
				break;
			case 4:
				row_view(conn);
				break;
			case 5:
				delete_product(conn);
				break;
			case 6:
				customize(conn);
				break;
			case 7:
				System.exit(0);
				break;
			default:
				System.out.println("Invalid choice");
				break;
			}
		
			}
		} 
		catch (ClassNotFoundException e) 
		{
			
			e.printStackTrace();
		}
		
		
	}
	public void view(Connection conn) throws SQLException
	{
		String qur="Select * from store";
		java.sql.Statement st=conn.createStatement();
		ResultSet rs=st.executeQuery(qur);
		System.out.println("Product id \tProduct name \tProduct price \tproduct quantity \tProduct expirty");
		while(rs.next())
		{
			System.out.println(rs.getInt(1)+"\t\t"+rs.getString(2)+"\t\t"+rs.getInt(3)+"\t\t"+rs.getInt(4)+"\t\t"+rs.getDate(5));
		}
	}
	public void new_input(Connection conn) throws SQLException
	{
		char ans;
		do
		{
		String qur="insert into store values(?,?,?,?,?)";
		PreparedStatement ps=conn.prepareStatement(qur);
		int p_id;
		String p_name;
		int p_price;
		int p_quantity;
		String p_expirty;
		System.out.println("Enter product id");
		p_id=sc.nextInt();
		System.out.println("Enter prouct name");
		p_name=sc.next();
		System.out.println("Enter product price in Rs.");
		p_price=sc.nextInt();
		System.out.println("Enter product quantity Kg.");
		p_quantity=sc.nextInt();
		System.out.println("Enter date of expirty");
		p_expirty=sc.next();
		Date expirty=Date.valueOf(p_expirty);
		ps.setInt(1,p_id);
		ps.setString(2, p_name);
		ps.setInt(3, p_price);
		ps.setInt(4, p_quantity);
		ps.setDate(5,expirty);
		int count=ps.executeUpdate();
		if(count>0)
		{
			System.out.println("Data added successfully");
		}
		System.out.println("do you want add another data ");
		System.out.println("Enter 'Y' for yes & 'N' for no");
		 ans=sc.next().charAt(0);
		}while(ans=='y'|| ans=='Y');
	}
	public void update(Connection conn) throws SQLException
	{
		int ch1;
		System.out.println("WHich item do you want to update");
		System.out.println("1:quantity");
		System.out.println("2:price");
		ch1=sc.nextInt();
		switch(ch1)
		{
		case 1:
			int p_id;
			int p_quantity;
			System.out.println("Enter Product id");
			p_id=sc.nextInt();
			System.out.println("Enter new quantity");
			p_quantity=sc.nextInt();
			String qur="update store set p_quantity=? where p_id=?";
			PreparedStatement ps=conn.prepareStatement(qur);
			ps.setInt(1,p_quantity);
			ps.setInt(2,p_id);
			ps.executeUpdate();
			System.out.println("Data updated successfully");
			break;
		case 2:
			int p_price;
			System.out.println("Enter Product id");
			p_id=sc.nextInt();
			System.out.println("Enter the new price of product");
			p_price=sc.nextInt();
			String qur1="update store set p_price=? where p_id=?";
			PreparedStatement ps1=conn.prepareStatement(qur1);
			ps1.setInt(1, p_price);
			ps1.setInt(2,p_id);
			ps1.executeUpdate();
			System.out.println("Data updated successfully");
			break;
		default:
			System.out.println("Sorry you enter invalid choice");
			break;
		}	
	  }
	public void row_view(Connection conn) throws SQLException
	{
		System.out.println("Enter Product id");
		p_id=sc.nextInt();
		String qur="select * from store where p_id=?";
		PreparedStatement pse=conn.prepareStatement(qur);
		pse.setInt(1, p_id);
		ResultSet rs=pse.executeQuery();
		System.out.println("Product id \tProduct name \tProduct price \tProduct quantity");
		while(rs.next())
		{
			System.out.println(rs.getInt(1)+"\t\t"+rs.getString(2)+"\t\t"+rs.getInt(3)+"\t\t"+rs.getInt(4));
		}
	}
	
	public void delete_product(Connection conn) throws SQLException
	{
		String confirm;
		System.out.println("Enter Product id");
		int p_id=sc.nextInt();
		System.out.println("Are you sure for deleting the entered record"
				+ "press Y for yes and N for No");
		confirm=sc.next();
		if(confirm.equalsIgnoreCase("Y"))
		{
		String qur="delete from store where p_id=?";
		PreparedStatement ps=conn.prepareStatement(qur);
		ps.setInt(1, p_id);
		ps.executeLargeUpdate();
		System.out.println("deleted successfully");
		}
		else
		{
			System.out.println("cancelling the deletion of record");
		}
	}
	public void customize(Connection conn) throws SQLException
	{
		char ans1;
		do 
		{
		System.out.println("which list do you want according to");
		System.out.println("1:Product Id");
		System.out.println("2:Product name");
		System.out.println("3:Product Quantity");
		System.out.println("4:Product Price");
		System.out.println("5:Exit");
		int ch2=sc.nextInt();
		switch(ch2)
		{
		case 1:
			System.out.println("Enter Product Id");
			p_id=sc.nextInt();
			String qur="select * from store where p_id=?";
			PreparedStatement pse=conn.prepareStatement(qur);
			pse.setInt(1, p_id);
			ResultSet rs=pse.executeQuery();
			System.out.println("Product id \tProduct name \tProduct price \tProduct quantity");
			while(rs.next())
			{
				System.out.println(rs.getInt(1)+"\t\t"+rs.getString(2)+"\t\t"+rs.getInt(3)+"\t\t"+rs.getInt(4));
			}
			break;
		case 2:
			System.out.println("Enter Product name");
			p_name=sc.next();
			String que="select * from store where p_name=?";
			PreparedStatement ps=conn.prepareStatement(que);
			ps.setString(1,p_name);
			ResultSet rs1=ps.executeQuery();
			System.out.println("Product id \tProduct name \tProduct price \tProduct quantity");
			while(rs1.next())
			{
				System.out.println(rs1.getInt(1)+"\t\t"+rs1.getString(2)+"\t\t"+rs1.getInt(3)+"\t\t"+rs1.getInt(4));
			}
			break;
		case 3:
			System.out.println("Enter Product quantity");
			p_quantity=sc.nextInt();
			String qui="select * from store where p_quantity=?";
			PreparedStatement psi=conn.prepareStatement(qui);
			psi.setInt(1,p_quantity);
			ResultSet re=psi.executeQuery();
			System.out.println("Product id \tProduct name \tProduct price \tProduct quantity");
			while(re.next())
			{
				System.out.println(re.getInt(1)+"\t\t"+re.getString(2)+"\t\t"+re.getInt(3)+"\t\t"+re.getInt(4));
			}
			break;
		case 4:
			System.out.println("Enter Product price");
			p_price=sc.nextInt();
			String qut="select * from store where p_price=?";
			PreparedStatement psp=conn.prepareStatement(qut);
			psp.setInt(1,p_price);
			ResultSet rw=psp.executeQuery();
			System.out.println("Product id \tProduct name \tProduct price \tProduct quantity");
			while(rw.next())
			{
				System.out.println(rw.getInt(1)+"\t\t"+rw.getString(2)+"\t\t"+rw.getInt(3)+"\t\t"+rw.getInt(4));
			}
			break;
		default:
			System.out.println("Sorry you entered Wrong choice");
			break;
		}
		System.out.println("Do you want another Customize list???");
		System.out.println("'Y' for Yes & 'N' for No");
		ans1=sc.next().charAt(0);
	  }while(ans1=='Y' || ans1=='y');
	 }
  	public static void main(String[] args) throws SQLException 
	{
		System.out.println("Welcome to the store management System");
		System.out.println("Lakshmi kirana");
		store obj=new store();
		obj.operations();
	}
}
