# Google Analytics YouTube Video Tracking
Tracking YouTube Player Events with Google Analytics.

*Note: This fork of the original allows for use with the newer analytics.js. The original is restricted to the older ga.js.*

## Usage
Include the scripts in the body section of the HTML document, just before the `</body>` tag. You’ll need to be running on a web server instead of opening the file directly in your browser. Flash and JS security restrictions will prevent the API from working when run locally.

### Basic
```html
<script src="path/to/youtube.ga.min.js"></script>
<script>
	var configYouTubePlayer = {
		videoID: 'Rk6_hdRtJOE' // The YouTube video ID that identifies the video that the player will load.
	};
</script>
```
### With some options
```html
<script src="path/to/youtube.ga.min.js"></script>
<script>
	var configYouTubePlayer = {
        // The YouTube video ID that identifies the video that the player will load.
        videoID: 'Rk6_hdRtJOE',

        // The height of the video player. The default value is 390.
        height: 390,

        // The width of the video player. The default value is 640.
        width: 640,

        // Enable progress event tracking.
        trackProgress: true,

        // Enable video quality event tracking.
        trackPlaybackQuality: true,

        // YouTube Player API Reference
        // @see https://developers.google.com/youtube/player_parameters?playerVersion=HTML5#Parameters
        playerVars: {
            'autohide': 1,
            'autoplay': 1,
            'rel': 0,
            'theme': 'light'
        }
	};
</script>
```

Put the following div element inside the body element. The script will replace the div element with an iframe tag.
```html
<div id="ytplayer"></div>
```
The iframe embeds a YouTube video player and the YouTube iframe API posts content to the iframe tag on your page. This approach allows YouTube to serve an HTML5 player rather than a Flash player for mobile devices that do not support Flash.

## Requirements
* Google Analytics Tracking Code (Universal Analytics, *analytics.js*) *Note: See [original version](https://github.com/sanderheilbron/youtube.ga.js) for ga.js)*
* The end user must be using a browser that supports the HTML5 postMessage feature. Most modern browsers support postMessage, though Internet Explorer 7 does not support it.

## Browser Support
Tested in Chrome (39), Firefox (15), Safari (5,6), IE (8,9). Also tested on iOS.

## Event Tracking
All player events are only tracked once, except for video quality events. Restarting the video will not reset the event trackers.

### Default event trackers
* Category: YouTube
* Action:
	* **Started video**: when the video starts playing.
	* **Paused video**: captured if it occurs before 92% of the video has been viewed. This limit is in place because the YouTube player fires a "pause" event immediately before the finish event.
	* **Completed video**: when the video reaches 100% completion.
* Label: URL of embedded video on YouTube.

#####Example
```js
ga('send', 'event' 'YouTube', 'Started video', 'http://www.youtube.com/watch?v=Rk6_hdRtJOE&feature=player_embedded', undefined, {'nonInteraction': 1});
ga('send', 'event', 'YouTube', 'Paused video', 'http://www.youtube.com/watch?v=Rk6_hdRtJOE&feature=player_embedded', undefined, {'nonInteraction': 1});
ga('send', 'event', 'YouTube', 'Completed video', 'http://www.youtube.com/watch?v=Rk6_hdRtJOE&feature=player_embedded', undefined, {'nonInteraction': 1});
```
### Progress event trackers

* Category: YouTube
* Action:
	* **25% Progress**: when the video reaches 25% of the total video time.
	* **50% Progress**: when the video reaches 50% of the total video time.
	* **75% Progress**: when the video reaches 75% of the total video time.
* Label: URL of embedded video on YouTube.

#####Example
```js
ga('send', 'event', 'YouTube', 'Played video: 25%', 'http://www.youtube.com/watch?v=Rk6_hdRtJOE&feature=player_embedded', undefined, {'nonInteraction': 1});
ga('send', 'event', 'YouTube', 'Played video: 50%', 'http://www.youtube.com/watch?v=Rk6_hdRtJOE&feature=player_embedded', undefined, {'nonInteraction': 1});
ga('send', 'event', 'YouTube', 'Played video: 75%', 'http://www.youtube.com/watch?v=Rk6_hdRtJOE&feature=player_embedded', undefined, {'nonInteraction': 1});
```

### Quality event trackers
The YouTube player fires a "quality" event at the start of the video and subsequent events when the user selects a quality option from the player. YouTube selects the most appropriate playback quality, which will vary for different users, videos, systems and other playback conditions.

* Category: YouTube
* Action:
	* **1080 HD Quality**
	* **720p HD Quality**
	* **480p Quality**
	* **360p Quality**
	* **240p Quality**
* Label: URL of embedded video on YouTube.

#####Example
```js
ga(['send', 'event', 'YouTube', 'Video quality: 1080p HD', 'http://www.youtube.com/watch?v=Rk6_hdRtJOE&feature=player_embedded', undefined, {'nonInteraction': 1});
ga('send', 'event', 'YouTube', 'Video quality: 720p HD', 'http://www.youtube.com/watch?v=Rk6_hdRtJOE&feature=player_embedded', undefined, {'nonInteraction': 1});
ga('send', 'event', 'YouTube', 'Video quality: 480p', 'http://www.youtube.com/watch?v=Rk6_hdRtJOE&feature=player_embedded', undefined, {'nonInteraction': 1});
ga('send', 'event', 'YouTube', 'Video quality: 360p', 'http://www.youtube.com/watch?v=Rk6_hdRtJOE&feature=player_embedded', undefined, {'nonInteraction': 1});
ga('send', 'event', 'YouTube', 'Video quality: 240p', 'http://www.youtube.com/watch?v=Rk6_hdRtJOE&feature=player_embedded', undefined, {'nonInteraction': 1});
```

### Bounce rate
The event trackers do not impact bounce rate of the page which embeds the video. The value of the nonInteraction parameter is set to `1`.

## Issues
Have a bug? Please create an issue here on GitHub!

## Changelog

### 0.4 (via fork) (December 23, 2014):
 * Changed from ga.js to analytics.js
 * Updated documentation.

### 0.3 (January 29, 2013):
 * Bug fixes.
 * Updated documentation.

### 0.2 (November 5, 2012):
 * Changed source to allow `http://` or `https://`
 * Changed from `player_api` to `iframe_api`.
 * Added option to pull in YouTube API `playerVars`.
 * Created namespace.
 * Restructured functions to be more efficient.
 * Code cleanup.

### 0.1 (September 12, 2012):
 * Initial release.

## License
Licensed under the MIT license.

Copyright (C) 2012-2013 Sander Heilbron, http://sanderheilbron.nl

Edits by Ali Karbassi, http://karbassi.com

Analytics.js edits by Patrick Beeson, http://patrickbeeson.com

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
