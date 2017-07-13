---
layout: post
title: Collapse to Tabs
---

<img src="public/images/collapsetabs.gif">

The new <a href="https://developer.android.com/tools/support-library/features.html">Material design support library</a> basically handles this feature. This is by the use of the <code>CoordinatedLayout</code> which can take any interactions like drags, swipes, flings, or any other gestures.
<p>When it comes to handling collapse to tabs on scrolling, <code>CoordinatedLayout</code> is placed as the root as follow</p>

{% highlight java %}
	<android.support.design.widget.CoordinatorLayout 
		xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    xmlns:tools="http://schemas.android.com/tools"
	    android:id="@+id/coordinator"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    tools:context="android.aaron.com.material.ui.CollapseToToolbarActivity">

    <android.support.v4.view.ViewPager
        android:id="@+id/pager"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">

    </android.support.v4.view.ViewPager>

    <android.support.design.widget.AppBarLayout
        android:id="@+id/appBarLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <android.support.v7.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="@color/primary"
            android:minHeight="?attr/actionBarSize"
            app:layout_scrollFlags="scroll|enterAlways"
            app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
            app:theme="@style/ThemeOverlay.AppCompat.Dark">

        </android.support.v7.widget.Toolbar>

        <android.support.design.widget.TabLayout
            android:id="@+id/tabLayout"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            app:layout_collapseMode="pin"
            app:layout_scrollFlags="enterAlways"
           >
        </android.support.design.widget.TabLayout>
    </android.support.design.widget.AppBarLayout>

	</android.support.design.widget.CoordinatorLayout>
{% endhighlight %}


<p>
	
Notice that <code>Toolbar</code> has an attribute <code>app:layout_scrollFlags="scroll|enterAlways"</code>. 
The <code>scroll</code> feature means that the <code>Toolbar</code> will be scrolling off the screen during upscrolling while the <code>enterAlways</code> means that it will 'enter' to its initial place during downscrolling. Hiding animations, when to hide and when to 'reveal' are all taken care of by the library.
</p>
<p>Tabs do scroll up and down but never 'disappear. This is done but using the following attributes</p>
{% highlight java %}
	app:layout_collapseMode="pin"
    app:layout_scrollFlags="enterAlways
{% endhighlight %}

<p>The former feature means that the tabs will be 'pinned' on top when scrolling upwards</p>
<p>And thats all</p>