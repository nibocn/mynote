# android四大核心组件

## Activity

**Activity三种状态：**
* 运行状态；
* 暂停状态；
* 停止状态；

**Activity生命周期：**

![](http://img.hb.aicdn.com/ca76d44f5128a045a225f3597fad78a811f0c70d142cd-8bSkqw_fw658)

**Activity跳转**

由一个Activity跳转到另一个Activity中。
```
Intent intent = new Intent(MainActivity.this, SecondActivity.class);
startActivity(intent);
```

**关闭当前Activity**
```
finish();
```

**Activity间的数据传递**

数据设置：
```
Intent intent = new Intent(MainActivity.this, SecondActivity.class);
EditText editText = (EditText) findViewById(R.id.edit_input);
System.out.println("textView:" + editText.getText());
intent.putExtra("txt", editText.getText());
Bundle bundle = new Bundle();
bundle.putSerializable("user", new Users(1, "小明", true));
intent.putExtras(bundle);

startActivity(intent);
```
在`MainActivity`中设置要传递给`SecondActivity`的数据。使用`Intent`对象的`putXXX`方法，可传递大部分的数据类型。如果传递的数据较多封装成了一个自定的对象，那么可以使用`Bundle`对象的`putSerializable`方法进行保存，然后将`Bundle`对象设置到`Intent`对象的`putExtras`方法上。**注：**`putSerializable`方法的参数要是可以被序列化的对象。

数据获取：
```
String txt = getIntent().getCharSequenceExtra("txt").toString();
Users users = (Users) getIntent().getExtras().getSerializable("user");

System.out.println("txt:" + txt);
System.out.println("user info:" + users.toString());
textView.setText(users.getUserName());
```
在`SecondActivity`中通过`getIntent`方法获取到`Intent`对象，然后通过`getXXX`方法填写相应的name获取对应的数据。

**返回当前Activity的数据到前一个Activity**
```
Intent intent = new Intent(MainActivity.this, SecondActivity.class);
EditText editText = (EditText) findViewById(R.id.edit_input);
System.out.println("textView:" + editText.getText());
intent.putExtra("txt", editText.getText());
Bundle bundle = new Bundle();
bundle.putSerializable("user", new Users(1, "小明", true));
intent.putExtras(bundle);

startActivityForResult(intent, 0);
```

```
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    System.out.println("requestCode:" + requestCode);
    System.out.println("resultCode:" + resultCode);
    TextView textView = (TextView) findViewById(R.id.txt_view);
    String result = data.getStringExtra("result");
    textView.setText(result);

    super.onActivityResult(requestCode, resultCode, data);
}
```
在`MainActivity`里面调用`startActivityForResult`方法打开`SecondActivity`。并且重写`onActivityResult`方法，获取下面返回的Activity数据。
```
btnClose.setOnClickListener(new View.OnClickListener() {
@Override
public void onClick(View view) {
    Intent intent = new Intent();
    intent.putExtra("result", "返回结果...");
    setResult(2, intent);

    finish();
}
});
```
在`SecondActivity`里面通过`Intent`对象设置要返回给前一个Activity的数据，并调用`setResult`方法。关闭当前Activity。

