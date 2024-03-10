- 👋 Hi, I’m @Thetalent140
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
Thetalent140/Thetalent140 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
packagecom.finalyearproject;
Import android 
importandroid.app.Activity;
importandroid.app.Dialog;
importandroid.content.Intent;
importandroid.os.Bundle;
importandroid.view.View;
importandroid.view.View.OnClickListener;
importandroid.widget.Button;
importandroid.widget.TextView;
importandroid.widget.Toast;

public class Homepage2 extends Activity implements OnClickListener{
	Button CTT,COT,VT;
	publicbooleantableCreated = false;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		// TODO Auto-generated method stub
		super.onCreate(savedInstanceState);
		setContentView(R.layout.homepage2);
		
		Intialize();
	}

	private void Intialize() {
		// TODO Auto-generated method stub
		CTT = (Button) findViewById(R.id.bCCTT);
		COT = (Button) findViewById(R.id.bCOT);
		VT = (Button) findViewById(R.id.bVT);		
		CTT.setOnClickListener(this);
		COT.setOnClickListener(this);
		VT.setOnClickListener(this);
	}

	@Override
	public void onClick(View v) {
		// TODO Auto-generated method stub
		switch(v.getId()){
		case R.id.bCCTT:
			tableCreated = true;
			Intent i = new Intent(Homepage2.this,Classtable.class);
			startActivity(i);
			break;
		case R.id.bVT:
			if(tableCreated==false){
				Toast.makeText(Homepage2.this, "Please Create a Table", Toast.LENGTH_LONG).show();
			}
			if(tableCreated==true){
			Intent k = new Intent(Homepage2.this, ViewTables.class);
			startActivity(k);
			}
			break;
		caseR.id.bCOT:
			tableCreated = true;
			Intent w = new Intent(Homepage2.this, ReminderListActivity.class);
			startActivity(w);
			break;
		}
	}
	

}




packagecom.finalyearproject;

importandroid.app.IntentService;
importandroid.content.Context;
importandroid.content.Intent;
importandroid.os.PowerManager;

public abstract class WakeReminderIntentService extends IntentService {
abstract void doReminderWork(Intent intent);
	
	public static final String LOCK_NAME_STATIC="com.dummies.android.taskreminder.Static";
	private static PowerManager.WakeLocklockStatic=null;
	
	public static void acquireStaticLock(Context context) {
		getLock(context).acquire();
	}
	
	synchronized private static PowerManager.WakeLockgetLock(Context context) {
		if (lockStatic==null) {
			PowerManager mgr=(PowerManager)context.getSystemService(Context.POWER_SERVICE);
			lockStatic=mgr.newWakeLock(PowerManager.PARTIAL_WAKE_LOCK,
														LOCK_NAME_STATIC);
			lockStatic.setReferenceCounted(true);
		}
		return(lockStatic);
	}
	
	publicWakeReminderIntentService(String name) {
		super(name);
	}
	
	@Override
	final protected void onHandleIntent(Intent intent) {
		try {
			doReminderWork(intent);
		}
		finally {
			getLock(this).release();
		}
	}
}










packagecom.finalyearproject;

importjava.util.ArrayList;
importjava.util.List;

importcom.google.gson.Gson;

importandroid.content.ContentValues;
importandroid.content.Context;
importandroid.database.Cursor;
importandroid.database.sqlite.SQLiteDatabase;
importandroid.database.sqlite.SQLiteDatabase.CursorFactory;
importandroid.database.sqlite.SQLiteOpenHelper;

public class Database {
	//variables for agenda table
	public static final String _id ="id_number";
	public static final String COURSE_CODE = "course";
	public static final String NAME = "name";
	public static final String DESCRIPTION = "description";
	public static final String START_AT = "started";
	public static final String END_AT = "ended";
	public static final String EXTRAS = "additional_info";
	public static final String GROUP_ID = "table_name ";
	public static final String LECTURER = "lecturer";
	public static final String BUILDING = "building";
	public static final String ROOM = "room_number";
	;
	
	//varaibles for groups table
	public static final String G_ID = "id_number";
	public static final String G_NAME = "table_name";
	public static final String G_CREATED_AT= "created";
	public static final String G_UPDATED_AT = "updated";
	
	private static final String DATABASE_NAME = "Iplan_DB";
	public static final String AGENDAS_TABLE = "Agenda";
	public static final String GROUPS_TABLE = "groups";
	private static final int DATABASE_VERSION =3;
	
	privateDBHelperourHelper;
	private  final Context ourContext;
	privateSQLiteDatabaseourDatabase;
		


private static class DBHelper extends SQLiteOpenHelper{

	publicDBHelper(Context context) {
		super(context, DATABASE_NAME, null, DATABASE_VERSION);
		
	}


	@Override
	public void onCreate(SQLiteDatabasedb) {
		//CREATE AGENDA TABLE
	db.execSQL("CREATE TABLE " + AGENDAS_TABLE + " (" + 
	_id + " INTEGER PRIMARY KEY AUTOINCREMENT, " +
	COURSE_CODE + " TEXT NOT NULL, " +
	NAME + " TEXT NOT NULL, " +
	DESCRIPTION + " TEXT NOT NULL, " + 
	START_AT + " TEXT NOT NULL, " +
	END_AT + " TEXT NOT NULL, " +
	BUILDING + " TEXT NOT NULL, " +
	LECTURER + " TEXT NOT NULL, " +
	ROOM + " TEXT NOT NULL, " +
	GROUP_ID + " INTEGER NOT NULL);"
			);
	
	//create groups table
	db.execSQL("CREATE TABLE " + GROUPS_TABLE + " (" + 
			G_ID + " INTEGER PRIMARY KEY AUTOINCREMENT, " +
			G_NAME + " TEXT NOT NULL);"
					);		
	}

	@Override
	public void onUpgrade(SQLiteDatabasedb, intoldVersion, intnewVersion) {
		db.execSQL("DROP TABLE IF EXISTS " + AGENDAS_TABLE);
		db.execSQL("DROP TABLE IF EXISTS " + GROUPS_TABLE);
		onCreate(db);	
	}
		
}
	public Database(Context c){
		ourContext = c;
	}
	
	public Database open(){
		ourHelper = new DBHelper(ourContext);
		ourDatabase = ourHelper.getWritableDatabase();
		return this;
	}
		public void close(){
			ourHelper.close();
		}

		

		/*public long createGroup(String forTables) {
			ContentValues cv = new ContentValues();
			cv.put(G_NAME, forTables);
			
			returnourDatabase.insert(GROUPS_TABLE, null, cv);
			
		}*/

		public List<Agendas>getAgendaData() {
			String columns[]={_id, NAME, DESCRIPTION, START_AT, END_AT, LECTURER,COURSE_CODE,BUILDING,ROOM};
			//ClassTimeTableGsoncGson = new ClassTimeTableGson();
			
			List<Agendas> agendas = new ArrayList<Agendas>();
			Cursor c = ourDatabase.query(AGENDAS_TABLE, columns, null, null, null, null, null);
			String result = "";
			intcid = c.getColumnIndex(_id);
			intcName = c.getColumnIndex(NAME);
			intcDescrip = c.getColumnIndex(DESCRIPTION);
			intcStart = c.getColumnIndex(START_AT);
			intcEnd = c.getColumnIndex(END_AT);
			//intcExt = c.getColumnIndex(EXTRAS);
			intcCode= c.getColumnIndex(COURSE_CODE);
			intcLect = c.getColumnIndex(LECTURER);
			intcBuild = c.getColumnIndex(BUILDING);
			intcRoom = c.getColumnIndex(ROOM);
			for(c.moveToFirst(); !c.isAfterLast(); c.moveToNext()){
				agendas.add(new Agendas(c.getInt(cid), c.getString(cName), c.getString(cDescrip), c.getString(cStart),
						c.getString(cEnd), 
						c.getString(cCode), 
						c.getString(cRoom),
						c.getString(cBuild),
						c.getString(cLect)
						
						)); 
			}
			return agendas;
		}

		public List<Group>getGroupData() {
			String columns[] = {G_ID, G_NAME};
			List<Group> groups = new ArrayList<Group>();
			Cursor c = ourDatabase.query(GROUPS_TABLE, columns, null, null, null, null, null);
			String result = "";
			intcID = c.getColumnIndex(G_ID);
			intcNAME = c.getColumnIndex(G_NAME);
			for (c.moveToFirst(); !c.isAfterLast(); c.moveToNext()){
				groups.add(new Group(c.getInt(cID), c.getString(cNAME)));
			}
			
			return groups;
		}
		
		public static class Group {
			public Integer id;
			public String name;
			
			public Group(Integer id, String name) {
				this.id = id;
				this.name = name;
			}
		}
		
		public static class Agendas{
			public Integer id;
			public String name;
			public String description, start_at, end_at, course_code,lecturer,room,building;
			public Agendas(Integer id, String name, String description,
					String start_at, String end_at, String course_code,
					String lecturer,String room, String building) {
				
				this.id = id;
				this.name = name;
				this.description = description;
				this.start_at = start_at;
				this.end_at = end_at;
				this.course_code = course_code;
				//this.extras = extras;
				this.building = building;
				this.lecturer= lecturer;
				this.room = room;
			}
			
			
		}
		public long createAgenda(String course, String lecturer2,
				String building2, String room2, String timeFrom, String timeTo,
				String courseCode, String courseDay) {
			ContentValues cv = new ContentValues();
			cv.put(NAME, course);
			cv.put(LECTURER, lecturer2);
			cv.put(BUILDING, building2);
			cv.put(ROOM, room2);
			cv.put(START_AT, timeFrom);
			cv.put(END_AT, timeTo);
			cv.put(COURSE_CODE, courseCode);
			cv.put(DESCRIPTION, courseDay);
			cv.put(GROUP_ID, 0);
		returnourDatabase.insert(AGENDAS_TABLE, null, cv);
		}
		
}



package com.finalyearproject;

import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

public class Homepage2 extends Activity implements View.OnClickListener {
    private boolean tableCreated = false;
    private Button CTT, COT, VT;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.homepage2);

        // Find views by their IDs
        CTT = findViewById(R.id.bCCTT);
        COT = findViewById(R.id.bCOT);
        VT = findViewById(R.id.bVT);

        // Set click listeners
        CTT.setOnClickListener(this);
        COT.setOnClickListener(this);
        VT.setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.bCCTT:
                tableCreated = true;
                Intent i = new Intent(Homepage2.this, Classtable.class);
                startActivity(i);
                break;
            case R.id.bVT:
                if (!tableCreated) {
                    Toast.makeText(Homepage2.this, "Please Create a Table", Toast.LENGTH_LONG).show();
                } else {
                    Intent k = new Intent(Homepage2.this, ViewTables.class);
                    startActivity(k);
                }
                break;
            case R.id.bCOT:
                tableCreated = true;
                Intent w = new Intent(Homepage2.this, ReminderListActivity.class);
                startActivity(w);
                break;
        }
    }
}



<?xmlversion="1.0"encoding="utf-8"?>
<ScrollView
xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_width="fill_parent"
android:layout_height="fill_parent">
<LinearLayout
android:layout_width="match_parent"
android:layout_height="match_parent"
android:orientation="vertical">



<EditText
android:id="@+id/etCourseCode"
android:layout_width="fill_parent"
android:layout_height="wrap_content"
android:hint="Enter Course Code"/>


<EditText
android:id="@+id/etEnterCourse"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:ems="10"
android:hint="Enter Course Title">
		
<requestFocusandroid:layout_width="match_parent"/>

</EditText>

<EditText
android:id="@+id/etLecturer"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:hint="Enter Lecturer Name"
android:ems="10"/>

<EditText
android:id="@+id/etBuilding"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:hint="Lecture Venue"
android:ems="10">
<requestFocusandroid:layout_width="wrap_content"/>

</EditText>

<EditText
android:id="@+id/etRoomNumber"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:hint="Enter Room Number"
android:ems="10"/>

<EditText
android:id="@+id/etCourseDay"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:hint="Course day"
/>

<Spinner
android:id="@+id/spinner1"
android:layout_width="fill_parent"
android:layout_height="fill_parent"
/>


<TextView
android:id="@+id/textView2"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:text="Course Time"
android:textAppearance="?android:attr/textAppearanceMedium"/>

<RelativeLayout
android:layout_width="fill_parent"
android:layout_height="100dip">

<EditText
android:id="@+id/etTo"
android:layout_width="70dip"
android:layout_height="wrap_content"
android:layout_centerVertical="true"
android:layout_marginLeft="22dp"
android:layout_toRightOf="@+id/textView3"
android:ems="10"
android:inputType="time"
android:hint="Click"/>

<TextView
android:id="@+id/textView4"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:layout_alignBaseline="@+id/etFrom"
android:layout_alignBottom="@+id/etFrom"
android:layout_centerHorizontal="true"
android:text="To"
android:textAppearance="?android:attr/textAppearanceMedium"/>

<EditText
android:id="@+id/etFrom"
android:layout_width="70dip"
android:layout_height="wrap_content"
android:layout_alignBaseline="@+id/etTo"
android:layout_alignBottom="@+id/etTo"
android:layout_alignParentRight="true"
android:layout_marginRight="36dp"
android:inputType="time"
android:ems="10"
android:hint="Click"/>

<TextView
android:id="@+id/textView3"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:layout_alignParentLeft="true"
android:layout_centerVertical="true"
android:text="From"
android:textAppearance="?android:attr/textAppearanceMedium"/>

</RelativeLayout>

<RelativeLayout
android:layout_width="fill_parent"
android:layout_height="60dip">

<Button
android:id="@+id/bSave"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:text="Save"/>

<Button
android:id="@+id/bview"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:layout_alignParentRight="true"
android:layout_alignParentTop="true"
android:layout_marginRight="20dp"
android:text="View"/>
</RelativeLayout>

</LinearLayout>
</ScrollView>
