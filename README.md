
# Assignment 6: Shared preferences
## Read data and show shared data from SecondActivity in MainActivity
Use Variables to declare SharedPreferences. During void onCreate() it is getting the 
SharedPreferences when the application starts.
During protected void resume() it is getting SharedPreferences every time a new activity 
starts inside the application.

## SecondActivity that can be opened from MainActivity
We can move between activities by using a button inside Main activity with intent that say we want to move to SecondActivity.
To move backwards we can add a parentActivityName inside the activity named Second activity in the Android manifest to mention that the parent of second Activity correspond Main Activity.

## Data to Shared Preferences using EditText
Inside Second activity we have a editText. The EditText value  correspond the value we send to our SharedPreference.
The EditText value saves by clicking the Save Button. The Save Button trigger a function called save.
The save function finds the value of EditText and stores the value inside the storage of the SharedPreferences
by using myPreferenceEditor.apply();.

## Code Examples
**Read Data**
```java
public class MainActivity extends AppCompatActivity {
    private SharedPreferences myPreferenceRef;
    private SharedPreferences.Editor myPreferenceEditor;

    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        myPreferenceRef = getSharedPreferences("key", MODE_PRIVATE);
        myPreferenceEditor = myPreferenceRef.edit();

        // Read a preference
        TextView prefTextRef = findViewById(R.id.EditTextDisplay);
        prefTextRef.setText(myPreferenceRef.getString("key", "No pref found."));
    }
}
```
_(onCreate)_
            
```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onResume() {
        super.onResume();
        TextView sharedPrefData = findViewById(R.id.EditTextDisplay);
        sharedPrefData.setText(myPreferenceRef.getString("key", "Name"));
    }
}
```
_(onResume)_

**Open SecondActivity**
```java
public class MainActivity extends AppCompatActivity {
    protected void onCreate(Bundle savedInstanceState) {

        Button submit = findViewById(R.id.NextPage);
        submit.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent(MainActivity.this, SecondActivity.class);
                startActivity(intent);
            }
        });
    }
}
```
_Navigation to SecondActivity_

```xml
<activity
            android:name=".SecondActivity"
            android:parentActivityName=".MainActivity"
            android:exported="false" 
/>
```
_Parent Button from SecondActivity to MainActivity_

**Data to Shared Preferences**
```java
public class SecondActivity extends AppCompatActivity {
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Button button = findViewById(R.id.SaveButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                save();
            }
        });
    }
}
```
_(Button To call save();)_

```java
public class SecondActivity extends AppCompatActivity {
    void save() {
        EditText sharedPrefData = findViewById(R.id.editText);
        myPreferenceEditor.putString("key", sharedPrefData.getText().toString());
        myPreferenceEditor.apply();
    }
}
```
_(Save Function)_

## Pictures
![](ShareData.png)
_activity_second.xml_

![](SharedData.png)
_activity_main.xml_
