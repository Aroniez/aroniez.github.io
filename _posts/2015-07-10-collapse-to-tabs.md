---
layout: post
title: Collapse to Tabs
---

<img src="public/images/collapsetabs.gif">

The new <a href="https://developer.android.com/tools/support-library/features.html">Material design support library</a> basically handles this feature. This is by the use of the <code>CoordinatedLayout</code> which can take any interactions like drags, swipes, flings, or any other gestures.
<p>When it comes to handling collapse to tabs on scrolling, <code>CoordinatedLayout</code> is placed as the root as follow</p>

~~~cpp
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
~~~cpp
