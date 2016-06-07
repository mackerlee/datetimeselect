# datetimeselect

android 65 lesson
1.TimePickerDialog时间选择对话框：在弹出的对话框里设置时间
    --a.创建一个类继承DialogFragment（3.0以后的版本才支持）;
    --b.重写onCreateDialog()方法，返回一个TimePickerDialog对象;
    --c.实现TimePickerDialog的onTimerSetListener接口来接受一个回调，当用户设置接收时间的响应.
