---
layout: post
title: 'Android Button 按键音'
categories:
- Android
tags:
- Android

---
##Android Button 按键音

{% highlight java linenos %}
public class MainActivity extends Activity {
	private SoundPool sp;// 声明一个SoundPool
	private int music;// 定义一个整型用load（）；来设置

	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		// 第一个参数为同时播放数据流的最大个数，第二数据流类型，第三为声音质量
		sp = new SoundPool(10, AudioManager.STREAM_SYSTEM, 5);
		//把你的声音素材放到res/raw里，第2个参数即为资源文件，第3个为音乐的优先级
		music = sp.load(this, R.raw.dot_audio, 1);
	}
	
	// 打点操作, 这个方法名要和xml布局文件<Button>属性android:onClick="dotBtnClick"一样
	public void dotBtnClick(View v) {
		// 打点声音
		sp.play(music, 1, 1, 0, 0, 1);
	}

	@Override
	protected void onStop() {
		super.onStop();
	}
}
{% endhighlight %}