# Audio Scripture Burrito Timing Format Proposal

The following repo contains examples of a proposed timing file format with USFM Markup. 

# Needs
Navigation through audio files requires two standards:
1. A file format for denoting timestamps to allow for seeking to specific sections of audio
2. A set of semantic tags to associate with timestamps to indicate what the audio represents at the associated timestamp

Initially, we propose a solution to allow for identifying and navigating to the following content within an audio file:
* Book Title
* Chapter Title
* Chapter content
* Section Heading
* Verse Number
* Verse Content
* Audio Footnotes

# Proposal
## WebVTT File Format
https://www.w3.org/TR/webvtt1/

https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API

The WebVTT File Format, while typically associated with Video, provides the following benefits:
* A registered mimetype: text/vtt (https://www.iana.org/assignments/media-types/text/vtt)
* Cues contain start and end
* Cues use timestamps of millisecond precision
* Wide adoption (supported by all major web browsers: https://caniuse.com/webvtt)
* Can potentially be used to address similar needs of video as well

## u23003 Biblical References for semantic tags
The needs for scripture in audio appear to be growing to include an audio equivalent of what exists with text (ie: an audio study Bible).
Additionally, while there exists a need for oral-only Scripture, many audio scripture burritos will match a text scripture burrito. As such,
it seems that the use of the u23003 Biblical Reference system addresses most if not all current needs of audio scripture, as well as allow for association with a matching
text USFM document where applicable. Furthermore, this allows for usage of the same referencing systems used with text.

We propose limiting the audio recorded to only tags which would be rendered to the screen for a text USFM document (this would include audio footnotes as 
footnotes are content which would be rendered). Metadata tags can still exist, though the beginning and ending timestamp should collapse to 0 time elapsed.

## Biblical References in WebVTT
WebVTT allows for many HTML tags to be used in the cue payload as well as with CSS styling. Included in this is the class tag `<c>`.
We propose the use of a class tag in order to use Biblical References within the timing file:

### <c.u23003>
The content within the u23003 class tag contains the reference. 

For example, a tag for Ephesians Chapter 1 Verse 1 would look like: `<c.u23003>EPH 1:1</c>`

Content of the USFM tag can optionally be included following this line. As the WebVTT file is used for captioning video, this can allow for use of the audio track with a video,
in which also displaying the corresponding text may be desireable. More concretely, if it is desired to include the audio of a chapter of scripture to be played alongside a corresponding
sign language video of the same scripture, the matching text could be provided here to be displayed as a caption.


For instances where the tag itself is read aloud in the the audio track, we use the "verse 0" convention of references. For instance, the audio of the words "Verse 1," 
in order to state audibly that the following audio is the content of Verse 1.

For example: `<c.u23003>EPH 1:1:0</c>`

### c.u23003 styling
As the contents of a VTT cue are intended to be displayed as a caption, the use of this field as semantic tags is undesirable if the VTT file is being used as intended alongside video.
Rather than require a separate VTT file solely for our purposes, our tags can coexist alongside other VTT content by use of the following CSS:

```
c.u23003 {
  display: none;
}
```
