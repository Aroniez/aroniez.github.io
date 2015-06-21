---
layout: post
title: Material Tabs Revisited
comments: true
---
This isn't a detailed tutorial on how to implement material tabs but just a quick revisit on'em. 
I assume you've created your project and everything is well set. Head over <a href="http://www.materialpalette.com/">Materialpallte</a> for awesome color choices and download an xml type of colors.

</p>
<p>With Toolbar aboard in replacement of ActionBar, lets start by adding it to our new layout. roughly that would translate to something like this.</p>
<img src="public/images/toolbar.png"  height="400px" width="900px">
<img src="public/images/toolbar.png" height="700px" width="350px">
<b>Then came material tabs</b>
For my lazy fellas we gotta head over to our browsers for two major classes, Mr. <code>SlidingTabLayout.java</code> and Mrs.<code>SlidingTabStrip.java</code>.
I like to get mine from Google IO app <a href="https://github.com/google/iosched/tree/master/android/src/main/java/com/google/samples/apps/iosched/ui/widget">Here</a> but you can also get them elsewhere. There is no rule that you should get them from somewhere but my laziness tells me so. 
Their names are self explanatory so lets see what happens under the hood.
<p>With no error nearby, head over to <code>xml</code> and add some lines
first, add SlidingTabLayout just below <code>Toolbar</code>. snippet for SlidingTablayout goes like this
<p>
<code>&lt;android.aaron.com.material.tabs.SlidingTabLayout
            android:id="@+id/tabs"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="@color/primary"&gt;
  &lt;/android.aaron.com.material.tabs.SlidingTabLayout&gt;</code>
	</p>


but watch out for the package name in <code>android.aaron.com.material.tabs.SlidingTabLayout</code>. Make sure you replace it with your package.

Now add <code>Viewpager</code> below them. Something like
<p>
<code> &lt;android.support.v4.view.ViewPager
                android:id="@+id/pager"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:layout_alignParentTop="true"&gt;
            &lt;/android.support.v4.view.ViewPager&gt;</code>
            </p>
</p>

<p>With xml ready to rock and roll, head back to java</p>
<p>Start by doing the <code>findViewById()</code> thing or use <a href="http://jakewharton.github.io/butterknife/">Butterknife</a> if you are farmiliar with it (You should actually use the later...). So far so good? Thats what I thought.</p>
<p><strong>Them Fragments</strong></p>
<p>I was thinking of using one fragment but then why not create seperate ones for diffrent purpose. Lets say we wanna create something like this... 
<img src="public/images/tabs.png" height="700px" width="350px">
<p>Create three fragments. A simple fragment class looks like this
<pre><code>public class PageTwo extends Fragment {

    public PageTwo() {
        // Required empty public constructor
    }

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        // Inflate the layout for this fragment
        View view = inflater.inflate(R.layout.fragment_page_two, container, false);
        return view;
    }

    }
}
</code></pre>
</p>
</p>
<p><b>Mr Adapter, where you at?</b></p>
<p>Create a class, name it something like MaterialTabsAdapter or something not weird. With simplicity kept constant, extend <code>FragmentPagerAdapter</code>
in the <code>getItem(int position)</code> method, you can get fragments by,
<pre><code>
 @Override
    public Fragment getItem(int position) {
      
        Fragment fragment;
        switch (position) {
            case 0:
                fragment = new PageOne();
                return fragment;

            case 1:
                fragment = new PageThree();
                return fragment;
            case 2:
                fragment = new PageTwo();
                return fragment;
            default:
                fragment = new PageOne();
                return fragment;
        }
    }
</code>
</pre>
<p>This means that when we are in a particular page we load a particular fragment</p>
<p>
<pre><code> @Override
    public int getCount() {
        //The number of fragments we got
        return 3;
    }

    @Override
    public CharSequence getPageTitle(int position) {
        //Sample titles for our tabs
        if (position == 0) {
            return "Movies";
        } else if (position == 1) {
            return "Series";
        } else if (position == 2) {
            return "Films";
        }
        return "";
    }
</code>
</pre>
</p>
<p>So far we've done nearly everything. The last lap is to switch the circuit on and watch</p>
<p><strong>Let there be magic...</strong></p>
<p>
Lets make this baby to walk on its own.
	Pull over from adapter and head to <code>MainActivity.java</code>, assuming that's the correct class and add the following</p>
<pre><code>MaterialTabsAdapter myFragmentAdapter = new MaterialTabsAdapter(getSupportFragmentManager());   
	viewPager.setAdapter(myFragmentAdapter);
	slidingTabLayout.setDistributeEvenly(true);
	slidingTabLayout.setViewPager(viewPager);
</code></pre>
<p>Easy right, Keep it simple Stupid (KISS).
	This was my first post and it's probably terrible. Too bad there ain't comment section, but if you feel like sharing something with me, ronolikeaaron@gmail.com is my mail. Cheers!
</p>

