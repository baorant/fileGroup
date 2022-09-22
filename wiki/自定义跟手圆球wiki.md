# 安卓开发自定义组件1（模拟浮动圆球跟手移动）
## 背景
在实际开发中，View类还不足以满足程序所有的需求。这时，便可以通过继承View类重写相应方法来开发自己的组件。
开发自定义组件的步骤：
1、创建一个继承android.view.View类的View子类，并且重写构造方法；
2、根据需要重写相应的方法(onDraw(),onMeasure等)；
3、创建并实例化自定义View类，并将其添加到布局管理器中。
## demo实践-模拟浮动圆球跟手移动
1、自定义view子类
```
public class BallView extends View {
    public float bitmipx;
    public float bitmipy;
    Paint paint;
    Bitmap bitmap;

    public BallView(Context context) {
        super(context);
        bitmipx = 550;
        bitmipy = 550;
        // 创建并实例化Paint对象
        paint = new Paint();
        //根据图片生成位图对象
        bitmap = BitmapFactory.decodeResource(this.getResources(), R.drawable.ball);
    }

    public BallView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public BallView(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        if (bitmap == null) {
            bitmap = BitmapFactory.decodeResource(this.getResources(), R.drawable.ball);
        }
        // 绘制图片
        canvas.drawBitmap(bitmap, bitmipx, bitmipy, paint);
        if(bitmap.isRecycled()) //判断图片是否可以回收
        {
            bitmap.recycle();  //强制回收图片
        }
    }
}
```
2、在需要展示的layout布局文件中引用自定义的view类
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".activity.BallActivity">
<com.baorant.test2.view.BallView
    android:id="@+id/ballView"
    app:layout_constraintLeft_toLeftOf="parent"
    app:layout_constraintRight_toRightOf="parent"
    app:layout_constraintTop_toTopOf="parent"
    app:layout_constraintBottom_toBottomOf="parent"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"/>
</androidx.constraintlayout.widget.ConstraintLayout>
```
3、activity中添加点击事件
```
public class BallActivity extends AppCompatActivity {
    BallView ballView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_ball);

        ballView = findViewById(R.id.ballView);
        ballView.setOnTouchListener(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View v, MotionEvent event) {
                ballView.bitmipx = event.getX();
                ballView.bitmipy = event.getY();
                // 重绘自定义组件
                ballView.invalidate();
                return true;
            }
        });
    }
}
```
## demo效果如下
<img src="https://github.com/baorant/fileGroup/blob/main/image/%E8%B7%9F%E6%89%8B%E8%BE%85%E5%8A%A9%E5%9C%86%E7%90%83.gif" alt="show" />