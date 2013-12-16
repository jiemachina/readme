[官网参考](http://developer.android.com/guide/topics/resources/animation-resource.html)

[参考二](http://developer.android.com/guide/topics/graphics/prop-animation.html#object-animator)

Android动画分为两大块：

* Property Animation(属性动画)

* View Animation(视图动画)

	此类又分为Tween animation和Frame animation

这两种动画的区别有：启动方式，改变本质；

视图动画只是改变外表；属性动画能够改变本质

###Property Animation(属性动画)

使用setTarget()
	
	AnimatorSet set = (AnimatorSet) AnimatorInflater.loadAnimator(myContext,
    R.anim.property_animator);
	set.setTarget(myObject);
	set.start();

ObjectAnimator是VauleAnimator的子类；

[参考](http://androidspace.duapp.com/?p=4421)  
VauleAnimactor会根据时间，值的范围，计算出一个计算因子（fraction），然后TypeEvaluator的evaluate计算出一个值，然后根据VauleAnimactor
	
	ValueAnimator animation = ValueAnimator.ofFloat(0f, 1f);# 值的范围
	animation.setDuration(1000);# 时间
	animation.addUpdateListener(new AnimatorUpdateListener() { #监听
    @Override
	publicvoid onAnimationUpdate(ValueAnimator animation) {
        Log.i("update", ((Float) animation.getAnimatedValue()).toString());
   	 	}
	});
	animation.setInterpolator(new CycleInterpolator(3));# 根据Interpolator计算值
	animation.start();
	

###View Animation(视图动画)

使用startAnimation

	ImageView spaceshipImage = (ImageView) findViewById(R.id.spaceshipImage);
	Animation hyperspaceJumpAnimation = AnimationUtils.loadAnimation(this, R.anim.hyperspace_jump);
	spaceshipImage.startAnimation(hyperspaceJumpAnimation);