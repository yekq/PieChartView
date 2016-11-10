# PieChartView
Android 可点击扇形图

##效果图:
![](https://github.com/yekq/Resource/blob/master/device-2016-05-02-173223.mp4_1462182411.gif)  

#Use:
Layout
```xml
    <ykq.demo.PieChartView.PieChartView
        android:id="@+id/pieChartView"
        android:layout_width="250dp"
        android:layout_height="270dp"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="100dp"
        app:lableTextSize="16dp"
        app:centreRadius="0dp"
        app:gravity="top"
        app:firstOffset="20dp"
        />
```
初始化
```java
        chartView.setFanClickAbleData(
                new double[]{75,15,60,24,90},
                new int[]{Color.GRAY,Color.GREEN, Color.DKGRAY,Color.GREEN,Color.BLUE},0.08);
```

动画
```java
        chartView.setOnFanClick(new OnFanItemClickListener() {
            @Override
            public void onFanClick(final FanItem fanItem) {
                    if (!fanRoateAniamtionStart)
                    {
                        float to;
                        float centre=(fanItem.getStartAngle() *2+ fanItem.getAngle())/2;
                        if (centre>=270)
                        {
                            to=360-centre+90;
                        }else
                        {
                            to=90-centre;
                        }

                        RotateAnimation animation= new RotateAnimation(0,to, chartView.getFanRectF().centerX(),chartView.getFanRectF().centerY());
                        animation.setDuration(800);
                        animation.setAnimationListener(new Animation.AnimationListener() {
                            @Override
                            public void onAnimationStart(Animation animation) {
                                fanRoateAniamtionStart=true;
                            }

                            @Override
                            public void onAnimationEnd(Animation animation) {
                                chartView.setToFirst(fanItem);
                                chartView.clearAnimation();
                                chartView.invalidate();
                                fanRoateAniamtionStart=false;
                                tv_select_item.setText("当前选中:"+ fanItem.getPercent() + "%");
                            }

                            @Override
                            public void onAnimationRepeat(Animation animation) {

                            }
                        });
                        animation.setFillAfter(true);
                        chartView.startAnimation(animation);
                    }
            }
        });
```
