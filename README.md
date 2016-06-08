# datetimeselect

android 65-66 lesson
1.TimePickerDialog时间选择对话框：在弹出的对话框里设置时间
    --a.创建一个类继承DialogFragment（3.0以后的版本才支持）;
    --b.重写onCreateDialog()方法，返回一个TimePickerDialog对象;
    --c.实现TimePickerDialog的onTimerSetListener接口来接受一个回调，当用户设置接收时间的响应.

2.TimePickerDialog实例：在res->layout->activity_main.xml中
    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:paddingBottom="@dimen/activity_vertical_margin"
        android:paddingLeft="@dimen/activity_horizontal_margin"
        android:paddingRight="@dimen/activity_horizontal_margin"
        android:paddingTop="@dimen/activity_vertical_margin"
        tools:context="com.example.mackerlee.android_65.MainActivity">
    
        <TextView
            android:id="@+id/tv_settime"
            android:layout_width="match_parent"
            android:layout_height="wrap_content" />
        //--点击该按钮弹出时间设置组件
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Set Time"
            android:id="@+id/button01"
            android:layout_below="@id/tv_settime"
            //--设置按钮响应事件
            android:onClick="setTimeClick"/>
        //--点击弹出日期设置按钮
         <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Set Date"
        android:id="@+id/button02"
        android:layout_below="@id/button01"
        android:onClick="setDateClick"/>
    </RelativeLayout>
    
    在java->包名->MainActivity.java中：
    package com.example.mackerlee.android_65;

    import android.app.Dialog;
    import android.app.TimePickerDialog;
    import android.support.v4.app.DialogFragment;
    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import android.text.format.DateFormat;
    import android.view.View;
    import android.widget.Button;
    import android.widget.TextView;
    import android.widget.TimePicker;
    import android.widget.Toast;
    
    import java.util.Calendar;
    
    public class MainActivity extends AppCompatActivity {
    
        private Button btnSetTime;
        private TextView tv_setTime;
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            btnSetTime = (Button)findViewById(R.id.button01);
            tv_setTime = (TextView)findViewById(R.id.tv_settime);
        }
    
        public void setTimeClick(View view){
            //--调用SetTimeDialog并new一个该内部类
            DialogFragment newFragment = new SetTimeDialog();
            //--参数：第一个是Fragment管理器，第二个是一个标记，只要是字符串的就行，用于以后管理获取该Dialog
            newFragment.show(getSupportFragmentManager(), "timePicker");
        }
        
         //--Button02设置日期响应处理方法
        public void setDateClick(View view){
            //--调用SetDateDialog并new一个该内部类
            SetDateDialog std = new SetDateDialog();
            //--参数：第一个是Fragment管理器，第二个是一个标记，只要是字符串的就行，用于以后管理获取该Dialog
            std.show(getSupportFragmentManager(), "datePicker");
        }
    
        //--创建一个静态内部类继承DialogFragment
        public static class SetTimeDialog extends DialogFragment
                                            implements TimePickerDialog.OnTimeSetListener {
    
            //--重写onCreateDialog()方法
            @Override
            public Dialog onCreateDialog(Bundle savedInstanceState) {
                // Use the current time as the default values for the picker
                final Calendar c = Calendar.getInstance();
                int hour = c.get(Calendar.HOUR_OF_DAY);
                int minute = c.get(Calendar.MINUTE);
    
                //--创建并返回一个TimePickerDialog对象，Create a new instance of TimePickerDialog and return it
                //--参数：getActivity()是对话框中获取上下文对象的方法,this是TimePickerDialog.OnTimeSetListener事件回调方法，
                //----本类中实现了TimePickerDialog.OnTimeSetListener就用this表示调用下面的onTimeSet回调方法
                return new TimePickerDialog(getActivity(), this, hour, minute,
                        DateFormat.is24HourFormat(getActivity()));
            }
    
            //--TimePickerDialog.OnTimeSetListener回调事件的方法
            public void onTimeSet(TimePicker view, int hourOfDay, int minute) {
                // Do something with the time chosen by the user
                //tv_setTime.setText(hourOfDay+":"+minute);
                Toast.makeText(getActivity(),"现在是"+hourOfDay+":"+minute,Toast.LENGTH_LONG).show();
                //静态类内部不能调用this？
            }
        }
    
        //--创建一个静态内部类继承DialogFragment:日期选择对话框
        public static class SetDateDialog extends DialogFragment
                implements DatePickerDialog.OnDateSetListener {
    
            //--重写onCreateDialog()方法
            @Override
            public Dialog onCreateDialog(Bundle savedInstanceState) {
                // Use the current time as the default values for the picker
                final Calendar c = Calendar.getInstance();
                int year = c.get(Calendar.YEAR);
                int month = c.get(Calendar.MONTH);
                int day = c.get(Calendar.DAY_OF_MONTH);
    
                //--创建并返回一个DatePickerDialog对象，Create a new instance of DatePickerDialog and return it
                //--参数：getActivity()是对话框中获取上下文对象的方法,this是TimePickerDialog.OnTimeSetListener事件回调方法，
                //----本类中实现了DatePickerDialog.OnDateSetListener就用this表示调用下面的onTimeSet回调方法
                return new DatePickerDialog(getActivity(), this, year, month, day);
            }
    
            //--DatePickerDialog.OnDateSetListener回调事件的方法,参数DatePicker view是该视图组件对话框
            @Override
            public void onDateSet(DatePicker view, int year, int monthOfYear, int dayOfMonth) {
                // Do something with the time chosen by the user
                //tv_setTime.setText(hourOfDay+":"+minute);
                Toast.makeText(getActivity(),"现在是"+year+"年"+monthOfYear+"月"+dayOfMonth+"日",Toast.LENGTH_LONG).show();
    
            }
    
        }
    
    }

3.创建DatePickerDialog日期选择对话框：实例在上面
    --a.创建一个类继承DialogFragment;
    --b.重写onCreateDialog()方法，返回一个TimePickerDialog对象;
    --c.实现DatePickerDialog的OnDateSetListener接口来接收一个回调，响应用户设置日期

