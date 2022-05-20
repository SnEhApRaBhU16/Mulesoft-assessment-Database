import java.sql.*;
public class database {
	static String st = "jdbc:sqlite:C:/sqlite/Movies.db";//location of new database to be created
	
	public static Connection connectdb(){/*a method to connect to existing db*/
		try {
			Class.forName("org.sqlite.JDBC");
			Connection conn=DriverManager.getConnection("jdbc:sqlite:temp.db");//connecting to an an exsiting database 'temp.db'
			System.out.println("successful connection");
			conn.close();
			return conn;
		}catch(Exception ex) {
			System.out.println("not connected");
			System.out.println(ex);
			return null;
		}
	}
	
	
	
	public static void createdb() /*method to create new database*/ {  
		   
    try {  
            Connection conn = DriverManager.getConnection(st);  
            if (conn != null) {  
                  
                System.out.println("A new database has been created.\n");  
            }  
   
        } catch (SQLException e) {  
            System.out.println(e);  
        }  
    }
	
	
	public static void createNewTable() /*Method to create new table*/ {   
                  
    String sql = "CREATE TABLE movie(MOV_NAME VARCHAR(20),ACTOR_NAME VARCHAR(20),ACTRESS_NAME VARCHAR(20),DIRECTOR VARCHAR(20),YEAR_OF_RELEASE INTEGER)";//create query
      
        try{  
            Connection conn = DriverManager.getConnection(st);  
            Statement stmt = conn.createStatement();  
            stmt.execute(sql);  //execute the query to create movie table
            System.out.println("Table created\n");
        } catch (SQLException e) {  
            System.out.println(e.getMessage());  
        }  
    }  
	
	
	
	public static void insert(String mov_name,String actor_name,String Actress_name,String Director,int Year) /*Method to insert
	 values into the database*/{   
		String str = "INSERT INTO movie VALUES(?,?,?,?,?)";  //insert query
		 try{  
	            Connection conn =DriverManager.getConnection(st);  
	            PreparedStatement pst = conn.prepareStatement(str); 
	            pst.setString(1, mov_name);  //parameterized query
	            pst.setString(2, actor_name);  
	            pst.setString(3, Actress_name);  
	            pst.setString(4, Director);  
	            pst.setInt(5, Year);  
	            pst.executeUpdate();  
	            System.out.println("Row inserted!");
	        } catch (SQLException e) {  
	            System.out.println(e);  
	        } 
	}
		 
	
	public static void queriesop() /*queries to retrive information from db*/{   
		     /*FIRST QUERY*/     
		    System.out.println("\n\n (QUERY OUTPUT OF SELECTING ALL ROWS IN MOVIE TABLE)\n\n") ;
		    String str = "SELECT * FROM movie";
		       
		        try{  
		            Connection conn = DriverManager.getConnection(st);  
		            Statement stmt = conn.createStatement();
		            
		            ResultSet  rs = stmt.executeQuery(str);
		            while(rs.next())
		            {
		              System.out.print(rs.getString(1)+"\t");  //First Column
		              System.out.print(rs.getString(2)+"\t");  //Second Column
		              System.out.print(rs.getString(3)+"\t");  //Third Column
		              System.out.print(rs.getString(4)+"\t");//Fourth Column
		              System.out.print(rs.getString(5)+"\n\n");//Fifth Column
		            }
		         
		        } catch (SQLException e) {  
		            System.out.println(e.getMessage());  
		        } 
		        /*SECOND QUERY*/
		        System.out.println("\n\n(QUERY OUTPUT OF SELECTING A MOVIE NAME BASED ON PARTICULAR ACTOR (RajKumar Rao))\n\n") ;
		        String str2 = "SELECT MOV_NAME FROM movie WHERE ACTOR_NAME='RajKumar Rao'";
			       
		        try{  
		            Connection conn = DriverManager.getConnection(st);  
		            Statement stmt = conn.createStatement();
		            
		            ResultSet  rs = stmt.executeQuery(str2);//resulting set of rows
		            while(rs.next())
		            {
		              System.out.print(rs.getString(1)+"\t");  //First Column
		            }
		         
		        } catch (SQLException e) {  
		            System.out.println(e.getMessage());  
		        }  
		    
		    
	    }   
    
	
	public static void main(String[] args) {
		connectdb();//call the method to connect to existing db
		
		createdb();//call the method to create new db
		
		createNewTable();//call the method to create new table in that db
		
    	insert("English Vinglish", "Mehdi Nebbou", "Shridevi", "Gauri Shinde", 2012);
    	insert("Login", "Himanshu Bhatt", "Radhika Roy", "Sanjeev Reddy", 2012);
    	insert("Secret Superstar", "Aamir Khan", "Zaira Wasim", "Advait Chandan", 2017);
    	insert("Chalaang", "RajKumar Rao", "Nushrat Bharucha", "Hansal Mehta", 2020);
    	insert("Baahubali", "Prabhas","Anushka Shetty","S S Rajamouli",2016);
    	insert("Dasvi","Abhishek Bachan","Yami Gautam","Tushar Jalota", 2022);//6 rows inserted
    	
    	queriesop();//call the method to perform queries
	}
}
