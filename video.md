# CS50 Video Player

CS50 Video Player is a web app at [video.cs50.io](https://video.cs50.io/) that synchronizes instructional videos (e.g., lectures) with screen recordings so that students can toggle between the two. (Videos must be hosted on [YouTube](https://www.youtube.com/).) The player also provides

* a searchable dropdown menu with hyperlinked captions, if the instructional video is public and has (English) subtitles, and
* a dropdown menu with hyperlinked chapters, if the instructional video's description includes a table of contents.

Whenever the player is paused, the browser's URL bar is updated with the timecode (in seconds) at which the video was paused, to facilitate linking to specific moments. And if a student closes the player's tab before finishing a video, the player remembers where they left off and resumes at that timecode when the video is re-visited (provided its URL does not have an explicit `start` parameter).

## API

CS50 Video Player supports URLs of the form

```
https://video.cs50.io/:id
```

where `:id` is the ID of an instructional YouTube video. URLs of that form additionally support, via GET, these HTTP parameters:

* `screen`, which, if present, is the ID of a screen recording to synchronize with the instructional video.
* `start`, which, if present, is a time, in seconds, at which to start playback. It must be non-negative, less than the duration of the video itself, and less than `end`, if present.
* `end`, which, if present, is a time, in seconds, at which to end playback (by pausing). It must be non-negative, less than the duration of the video itself, and greater than `start`, if present.
* `offset`, which, if present, is a time, in milliseconds, with an optional plus sign or negative sign, by which to offset the screen recording from the instructional video.
    * If you started recording the screen before you started recording the instructional video, `offset` should be negative. For instance, if you started recording the screen at 12:00, and you started recording the instructional video at 12:01, then `offset` should be `-1000`.
    * If you started recording the screen after you started recording the instructional video, `offset` should be positive. For instance, if you started recording the instructional video at 12:00, and you started recording the screen at 12:01, then `offset` should be `1000` (or `+1000`).
* `mute`, which, if present with a value of `1`, will mute the instructional video (and screen recording, if any) by default.

For example, <https://video.cs50.io/5azaK2cBKGw?screen=byyRAKSo_dM&start=438>.

## Captions

If an instructional video has (English) captions, they will be automatically imported from YouTube.

## Chapters

If an instructional video has a table of contents in its description, it will be automatically imported as chapters from YouTube. A table of contents is defined as two or more lines in a video's description formatted as

```
00:00 - Chapter
MM:SS - Chapter
MM:SS - Chapter
...
```

or

```
00:00:00 - Chapter
HH:MM:SS - Chapter
HH:MM:SS - Chapter
...
```

where `Chapter` is the chapter's title.

Per [YouTube](https://support.google.com/youtube/answer/9884579),

* the first timestamp must be 00:00 (or 00:00:00),
* there should be at least three timestamps in ascending order, and
* the minimum length for chapters is 10 seconds.

The chapters will not appear along the player's timeline, as they do on YouTube itself, as YouTube does not (yet!) support chapters for embedded videos.

## Acknowledgements

Special thanks to CS50's friends at [Google](https://www.google.com/) for their support of this app and to Twitter for its [emojis](https://twemoji.twitter.com/)!
