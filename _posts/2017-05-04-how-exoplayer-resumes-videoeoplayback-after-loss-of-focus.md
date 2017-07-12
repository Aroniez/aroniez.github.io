---
layout: post
title: "How ExoPlayer Resumes Video Playback After Loss of Focus"
permalink: how-exoplayer-resumes-video-playback-after-loss-of-focus
date: 2017-06-04 13:07:14
comments: true
description: "How ExoPlayer Resumes Video Playback After Loss of Focus"
keywords: ""
categories:

tags:

---
After attending the most recent [Android Developers Meetup](https://www.meetup.com/nyandroiddevelopers/) in New York City and hearing about ExoPlayer2 (a great talk given by [@kylevenn]( https://twitter.com/kylevenn?lang=en)!) I really wanted to implement it in my app. ExoPlayer is an alternative to the basic `MediaPlayer` provided by the Android framework. In short, ExoPlayer is MUCH more extensible and customizable than `MediaPlayer`. The basic building blocks of ExoPlayer, `MediaSource` , `TrackSelector`, and `LoadControl` can call be customized to suit your needs. For more on this check out Google's [ExoPlayer Developer Guide](https://google.github.io/ExoPlayer/guide.html).

For my app I really didn't need anything too fancy, but I wanted to learn ExoPlayer and get familiar with it. I figured using the default implementations of the various components would be just fine. After implementing the most barebones version of ExoPlayer I quickly realized that when navigating away from my app during video playback and coming back resulted in a black screen, not exactly ideal. I needed the video to resume playing from where I left off.

Luckily ExoPlayer is open source and also provides a demo application in their repo. So after poking around the [PlayerActivity](https://github.com/google/ExoPlayer/blob/release-v2/demo/src/main/java/com/google/android/exoplayer2/demo/PlayerActivity.java) it became apparent how this is done.

When an Activity is no longer visible to the user the `onStop()` method is called, if a user presses the home button, keeping the Activity in memory, then `onStop()` is the last event in this callback chain. If the user were to press the back button instead the Activity would be destroyed, calling `onDestroy()` and then `onCreate()` again when the user goes back to a given Activity, in this case we would not want to save video playback since the user has completely exited the Activity and came back, potentially loading an entirely different video. However, in the case of hitting the Home button it is possible the user simply wanted to check something quick in another app and come back to the video they were watching. Here `onDestroy()` and `onCreate()` would not be called since the Activity remains in memory.

In the `onStop()` code of `PlayerActivity` `releasePlayer()` is called.

{% highlight java %}
private void releasePlayer() {
  if (player != null) {
    debugViewHelper.stop();
    debugViewHelper = null;
    shouldAutoPlay = player.getPlayWhenReady();
    updateResumePosition();
    player.release();
    player = null;
    trackSelector = null;
    trackSelectionHelper = null;
    eventLogger = null;
  }
}
{% endhighlight %}

In `releasePlayer()` the `updateResumePosition()` method is called.

{% highlight java %}
private void updateResumePosition() {
  resumeWindow = player.getCurrentWindowIndex();
  resumePosition = player.isCurrentWindowSeekable() ? Math.max(0, player.getCurrentPosition())
      : C.TIME_UNSET;
}
{% endhighlight %}

Here the player window and video position are saved to global variables.

Then, when the Acitivity comes back into the user's view `onStart()` is called, in `onStart()` the `initializePlayer()` method is called which contains some logic to check the `resumeWindow` variable:

{% highlight java %}
boolean haveResumePosition = resumeWindow != C.INDEX_UNSET;
if (haveResumePosition) {
  player.seekTo(resumeWindow, resumePosition);
}
{% endhighlight %}

This will check `resumeWindow` to see if it is not the default value ([C.INDEX_UNSET](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/C.html#INDEX_UNSET)) and if not it will load the saved window and seek to the saved position. And that's it! The video has now been resumed where the user left off.

Also note that `clearResumePosition()` is called in `onCreate()` because `onCreate()` will only get called again once the Activity gets destroyed and recreated. In this case we do not want to resume playback.
