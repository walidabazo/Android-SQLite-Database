# Android-SQLite-Database
# sqlite in android using eclipse


[![Watch the video](https://img.youtube.com/vi/FJPCXsJcImw/0.jpg)](https://youtu.be/FJPCXsJcImw)

Subscriber youtube channel 
https://www.youtube.com/channel/UCNJVG9_IebHe-NF-K_Y8Grw?sub_confirmation=1

## Table 
  | ID | Name | phone | Address | longitude | Latitude |
  | --- | --- | --- | --- | --- |--- |
  | 1 | Flavor Bar 1 | 0100000000 | 4 Shikolany St, Shubra, Cairo Governorate | 30.091485  | 31.323500 |
  | 2 | Flavor Bar 2 | 0200000000 | 18 Al Somal, El-Montaza, Heliopolis, Cairo Governorate | 30.074916  | 31.245592 |
  | 3 | Flavor Bar 3 | 0300000000 | Tiba Outlet Mall, 11371, 75 El-Nasr Rd | 30.067643  | 31.330085 |

## Following is the modified content of display Places activity 

    public class Database_Places 
     {
     int _ID;
     String  _name;
     String _phone;
    String  _Address;
    String  _longitude;
     String  _Latitude;


  
    public Database_Places(){
	
     }

    public Database_Places( int ID,String name,String phone,String Address,String longitude, String Latitude)
     {
       this._name =  name;
       this._phone= phone ;
       this._Address = Address ;
       this._longitude = longitude ;
       this._Latitude =  Latitude;
    }

    public Database_Places(String name,String phone,String Address,String longitude, String Latitude)

     {
     this._name =  name;
     this._phone = phone ;
     this._Address = Address ;
     this._longitude = longitude ;
     this._Latitude =  Latitude;
     }
    //get

     public int getID(){
     return this._ID;
     }

     public String getname(){	
     return this._name;

     }
     public String getphone(){	
     return this._phone;

     }

     public String getAddress(){	
     return this._Address;
     }
     public String getlongitude(){	
     return this._longitude; 
     }
     public String getLatitude(){
     return this._Latitude;
     }
     //set
     public void setID(int ID){	this._ID = ID ;  }
     public void setname(String name){	this._name = name;  }
     public void setphone(String phone){	this._name = phone;  }
     public void setAddress(String Address){	this._Address = Address ;  }
     public void setlongitude(String longitude){	this._longitude = longitude ;  }
     public void setLatitude(String Latitude){	this._Latitude = Latitude ;  }
     }
  
  ## Following is the content of Database class DBHelper.java
       import android.content.ContentValues;  
       import android.content.Context;  
       import android.database.Cursor;
       import android.database.DatabaseUtils;
       import android.database.sqlite.SQLiteDatabase;
       import android.database.sqlite.SQLiteDatabase.CursorFactory;
       import android.database.sqlite.SQLiteOpenHelper;  
       import java.util.ArrayList;  
       import java.util.List;  

    public class DatabaseHandler_Places  extends SQLiteOpenHelper {
   	private static final int DATABASE_VERSION = 1;  
    private static final String DATABASE_NAME = "wonderdeveloper";  
    private static final String TABLE_Places = "Places";  
    private static final String KEY_ID =   "ID";  
    private static final String KEY_name =   "name "; 
    private static final String KEY_phone =   "phone ";
    private static final String KEY_Address =   "Address";  
    private static final String KEY_longitude =   "longitude";  
    private static final String KEY_Latitude =   "Latitude";  
    public DatabaseHandler_Places(Context context) {
	  super(context, DATABASE_NAME, null, DATABASE_VERSION);  
		// TODO Auto-generated constructor stub
	}

	@Override
	public void onCreate(SQLiteDatabase db) {
		// TODO Auto-generated method stub
		String CREATE_CONTACTS_TABLE = "CREATE TABLE " + TABLE_Places + "("
				+ KEY_ID + " INTEGER PRIMARY KEY," 	+KEY_name + " NVARCHAR(255), "
        +KEY_phone + " NVARCHAR(255), " 
        	+KEY_Address + " NVARCHAR(255), " 
						+KEY_longitude + " NVARCHAR(255), " 
						+KEY_Latitude + " NVARCHAR(255)"+ ")";
		db.execSQL(CREATE_CONTACTS_TABLE);
	}
  
    @Override
	 public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
		// TODO Auto-generated method stub
		db.execSQL("DROP TABLE IF EXISTS " + TABLE_Places);
	      onCreate(db);
	}
	
      // code to get the single contact  
	Database_Places getContact(int id) {  
       SQLiteDatabase db = this.getReadableDatabase();  

        Cursor cursor = db.query(TABLE_Places, new String[] { KEY_ID,  
    KEY_name,
    KEY_phone,
    KEY_Address,
        		KEY_longitude,  }, KEY_ID + "=?",  
               new String[] { String.valueOf(id) }, null, null, null, null);  
 
        		if (cursor != null)  
           cursor.moveToFirst();  
  
       Database_Places contact = new Database_Places(Integer.parseInt(cursor.getString(0)),  
               cursor.getString(1), cursor.getString(2), cursor.getString(3), cursor.getString(4), cursor.getString(5)
              );  
        return contact;  
     }  
  
	// code to add the new contact  (ADD)
    void addContact(Database_Places contact) {  
        SQLiteDatabase db = this.getWritableDatabase();  
  
        ContentValues values = new ContentValues();  

  
        values.put(KEY_name,contact.getname() ); 
        values.put(KEY_phone,contact.getphone()); 
        
       
        values.put(KEY_Address,contact.getAddress()); 
        values.put(KEY_longitude,contact.getlongitude()); 
        values.put(KEY_Latitude,contact.getLatitude()); 
      
        // Inserting Row  
        db.insert(TABLE_Places, null, values);  
        //2nd argument is String containing nullColumnHack  
        db.close(); // Closing database connection  
    }  
    
     public boolean updateContact (Integer id, String name, String phone, String address, String longitude,String Latitude) {
        SQLiteDatabase db = this.getWritableDatabase();
        ContentValues contentValues = new ContentValues();
        contentValues.put("name", name);
        contentValues.put("phone", phone);
        contentValues.put("address", address);
        contentValues.put("longitude", longitude);
        contentValues.put("Latitude", Latitude);
        db.update(TABLE_Places, contentValues, "id = ? ", new String[] { Integer.toString(id) } );
        return true;
     }

     public Integer deleteContact (Integer id) {
        SQLiteDatabase db = this.getWritableDatabase();
        return db.delete(TABLE_Places,   "id = ? ", 
        new String[] { Integer.toString(id) });
     } 
     
    // code to get all contacts in a list view  
    public List<Database_Places> getAllContacts() {  
        List<Database_Places> contactList = new ArrayList<Database_Places>();  
        // Select All Query  
        String selectQuery = "SELECT  name,phone,Address,longitude,Latitude FROM " + TABLE_Places;  
  
        SQLiteDatabase db = this.getWritableDatabase();  
        Cursor cursor = db.rawQuery(selectQuery, null);  
  
        // looping through all rows and adding to list  
        if (cursor.moveToFirst()) {  
            do {  
            	Database_Places contact = new Database_Places();  
              
                contact.setname(cursor.getString(0));
                contact.setphone(cursor.getString(1));
                  contact.setAddress(cursor.getString(2));
                contact.setlongitude(cursor.getString(3));
                contact.setLatitude(cursor.getString(4));
         
                
                // Adding contact to list  
                contactList.add(contact);  
            } while (cursor.moveToNext());  
        }  
  
        // return contact list  
        return contactList;  
    }  
     //numberOfRows
    public int numberOfRows(){
        SQLiteDatabase db = this.getReadableDatabase();
        int numRows = (int) DatabaseUtils.queryNumEntries(db, TABLE_Places);
        return numRows;
     }
    //search by id
    public Cursor getData(int id) {
    SQLiteDatabase db = this.getReadableDatabase();
    Cursor res =  db.rawQuery( "select * from "+TABLE_Places+" where id="+id+"", null );
    return res;
     }
    }
## Following is the content of the modified Databas insert  places_insert_MainActivity.java.

    import android.app.Activity;
    import android.content.Intent;
    import android.os.Bundle;  
    import android.util.Log;
    import android.view.Menu;
    import android.view.MenuItem;

     import java.util.List;  

     public class Databasinsert_places extends Activity  {
  	@Override
	protected void onCreate(Bundle savedInstanceState) {
 		// TODO Auto-generated method stub
		super.onCreate(savedInstanceState);
		setContentView(R.layout.databasinsert_places);
	     
	  DatabaseHandler_Places db = new DatabaseHandler_Places(this);  
			  
	     
	     
	        db.addContact(new Database_Places ("Flavor Bar 1","0100000000", "4 Shikolany St, Shubra, Cairo Governorate" , "30.091485" , "31.323500"));
	      db.addContact(new Database_Places ("Flavor Bar 2 ","0200000000", "18 Al Somal, El-Montaza, Heliopolis, Cairo Governorate" , "30.074916" , "31.245592"));
	      db.addContact(new Database_Places ("Flavor Bar 3","0300000000", "Tiba Outlet Mall, 11371, 75 El-Nasr Rd, Al Manteqah Al Oula, Nasr City, Cairo Governoratet" , "30.067643" , "31.330085"));
	     Log.d("Reading: ", "Reading all contacts..");  
	        List<Database_Places> contacts = db.getAllContacts();  
	  
	        for (Database_Places cn : contacts) {  
	            String log = "ID: " + cn.getID() +"getname:"+ cn.getname()
	          + " ,getLatitude: " + cn.getLatitude()+", getLatitude:" + cn.getLatitude()
	          + " ,getphones: " + cn.getphone() 
	            + " ,getAddress: " + cn.getAddress();
	        
	            // Writing Contacts to log  
	            Log.d("Name: ", log);  
	          
	      
	       
	        }
	    
	  
	}


## Can be start web Augmented reality

Https://Webxr.edafait.com

## Good Company hosting and low price VPN 
https://shorturl.edafait.com/?fZVHLor 

## YouTube Channel Wonder developer To Subscriber 
https://shorturl.edafait.com/?zuS4kvW

