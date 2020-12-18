[![Generic badge](https://img.shields.io/badge/Made%20with-Python-<COLOR>.svg)](https://shields.io/)
![GitHub issues](https://img.shields.io/github/issues/shekharchander/tube_dl?color=red)
[![PyPI download month](https://img.shields.io/pypi/dm/tube-dl?color=blue)](https://pypi.org/project/tube_dl)
![GitHub followers](https://img.shields.io/github/followers/shekharchander?label=Follow&style=social)


# tube_dl

Tube_dl is a Simple Youtube video downloader for Python.
A Modular approach to bypass and download Youtube Videos and Playlist from Youtube using python.

  ```python 
  >>> pip install tube_dl
  ```

# Features:
### What's New (v4.1.0) :
   1. [Merge audio and video file.](#merging-formats)
   2. [Get links for streamable file (for Live videos)](#live) --> m3u8 file
### Existing Features
1. [Convert to mp3 or mp4](#convert) (Requires Moviepy - pip install moviepy)
2. [Fetch Comments](#comments)
3. [Fetch Captions](#captions)


# Usage:

```python
>>>from tube_dl import Youtube
>>>yt = Youtube("https://youtube.com/watch?v=R2j46bHm6zw&list=RDAMVMd4HYhxlsj5k")
>>>yt.formats.first().download()
```
## What is Formats?

formats is a class containing all the Youtube streams.

You can also see all the streams by : 

```python
>>> Youtube("https://youtube.com/watch?v=R2j46bHm6zw&list=RDAMVMd4HYhxlsj5k").Formats()
```
** Note that Formats() and formats are different.
When you use Formats(), a list of all streams of format class is returned.
When you use formats, a list of all streams of list_formats class is returned.

## Other Details
### Printing Information about the Video:

You can print the info or use them anywhere in your code. Here are the list of Information available:
  
   ```python
   
  >>> yt.title # Returns the title of the video
  >>> yt.views # Return total views
  >>> yt.channelName # Returns the name of the channel
  >>> yt.views # Returns total number of views
  >>> yt.likes # Returns total likes
  >>> yt.dislikes # Returns total dislikes
  >>> yt.subscribers # Returns no. of subscribers
  >>> yt.meta # Returns the metadata about video (Songs specifically) 
  >>> yt.channelUrl # Returns url of the channel
  >>> yt.channelThumb # Thumbnail of channel
  >>> yt.channelName # Returns name of the channel
  >>> yt.length # Returns the total length of the Youtube Video (in seconds)
  >>> yt.uploadDate # Returns the upload date of the video
  >>> yt.description # Return long description
  >>> yt.keywords # Returns list of keywords if available
  >>> yt.is_live # Return True if format is a live stream
  >>> yt.thumbnail # Returns thumbnail URL
  >>> yt.category # Category of the video
  >>> yt.dashUrl # Return dashStreamingUrl (if it is a live video)
  >>> yt.hlsUrl # Return hlsStreamingUrl (if it is a live video) 
  ```

### using filter_by option:
  Returns : list(list_formats)
  You can filter the formats according to the itag, adaptive, progressive, fps, quality, only_audio, no_audio.
  Example:
  ```python
  >>>Youtube("https://youtube.com/watch?v=R2j46bHm6zw&list=RDAMVMd4HYhxlsj5k").formats.filter_by(only_audio=True)
  ```
  
### Other options:
  ```python
  >>> yt.first()
  ```
    Returns the first index of list_formats
  ```python
  >>> yt.last()
  ```
    Returns the last index of list_formats
    
   Example:
  ```python
  >>> Youtube("https://youtube.com/watch?v=R2j46bHm6zw&list=RDAMVMd4HYhxlsj5k").formats.filter_by(only_audio=True).first()
  ```

  
### Downloading a format:

To download a format, .download() function is used. 
Params :
  Download takes following parameters. All are optional
  .download(convert,onprogress,path,file_name)
  1. convert takes a string as an argument. It converts the video into the extension you want. Ex: 'webm' -> 'mp3'. [mp4 coming soon]   
  2. onprogress takes function name as an argument. The function should have three arguments: Ex: def show_progress(Chunk=None,bytes_done=None,total_bytes=None)
  3. path takes full path where you want to save file
  4. file_name takes name of the file. by default, it is .title of the video. It is then processed to safe_filename to strip any invalid character.
  
 * You can print final filename by using formats.safe_filename() 
 Ex: 
 ```python
 >>> filename = Youtube("https://youtube.com/watch?v=R2j46bHm6zw&list=RDAMVMd4HYhxlsj5k").formats.safe_filename()
  ```
 
## Working with Playlist 


This Class is responsible for:
  1. Get list of all the Videos
  2. Create Continuation URL if len(videos)>100
  3. Get Continuation data and append all the video IDs to IDS variable
        
  Parameters : 
      url: str - URL of the PlayList
      start- Define start index of Videos
      end - Defines end index of videos.
      
  Returns :
      Tuple : All the Video IDs within the Range variable( if Defined)
```python
  >>> from tube_dl import Playlist, Youtube
  >>> pl = Playlist('https://music.youtube.com/playlist?list=PLTy__vzNAW6C6sqmp6ddhsuaLsodKDEt_').videos
  >>> for i in pl:
  >>>   yt = Youtube(f'https://youtube.com/watch?v={i}')
  >>>   yt.formats.first().download()
  ```

## <a name="caption">Captions</a>

Now you can download captions from youtube.
Here's the Sample code.

```python
>>> from tube_dl captions import Captions
>>> caption = Captions('url',language='en') # Use Captions('url').caption_details to get list of languages
>>> caption.fetch_captions() #raw xml output of captions
>>> caption.convert_to_srt(path='c://xample_path//',file_name='captions.srt') # Default filename is youtube id and default path is os.getcwd()
```

## <a name="comments">Comments</a>

Yes! It's possible. You can also download comments for a youtube video. It's still in beta but works absolutely file. Here's a simple use case of that.
```python
>>> from tube_dl.comments import Comments
>>> comment = Comments('Your Youtube URL').process_comments(count=45) # Don't define count variable to get all the comments.

```
* Fetching Replies for comments are not available yet. But will be there soon. Feel free to raise issues tickets.

## <a name="convert">Convert Formats</a>

Converting any format to `mp3` and `mp4` is easy. Here's how to do it:

### converting to mp3
```python
>>> from tube_dl import Youtube, extras
>>> yt = Youtube("https://youtube.com/watch?v=R2j46bHm6zw&list=RDAMVMd4HYhxlsj5k").formats.filter_by(only_audio=True)[0]
>>> b = a.download() #b variable stores the filename and meta(if available) as object of Output class.
>>> extras.Convert(b,'mp3',add_meta=True) #this will convert the format to mp3 and add meta if var add_meta is True
```
### converting to mp4

```python
>>> from tube_dl import Youtube, extras
>>> yt = Youtube("https://youtube.com/watch?v=R2j46bHm6zw&list=RDAMVMd4HYhxlsj5k").formats.filter_by(only_audio=True)[0]
>>> b = a.download() #b variable stores the filename and meta(if available) as object of Output class.
>>> extras.Convert(b,'mp4',kepp_original=True) #this will convert the format to mp4 and add_meta is not available for mp4 files.
#if keep_original is True, previous format will be deleted i.e. the file downloaded
```

## <a name="merging-formats">Merging Formats</a>
Please note that the merge should be between an audio and a video file. Merging speed depends on file size and your system processing speed and it completely depends on CPU performance.
```python
>>> from tube_dl import Youtube, extras
>>> yt = Youtube("https://youtube.com/watch?v=R2j46bHm6zw&list=RDAMVMd4HYhxlsj5k")
>>> yt1 = Youtube("https://youtube.com/watch?v=R2j46bHm6zw&list=RDAMVMd4HYhxlsj5k")
>>> video=yt.formats.filter_by(adaptive=True)[0].download()
>>> audio = yt1.formats.filter_by(only_audio=True)[0].download()
>>> extras.Merge(audio=audio,video=video,result='output.mp4',keep_original=False) 

```
The result variable stores the output filename and if keep_original = False, it will delete the raw files keeping only the output file.

## <a name="live">Working with Live Streams</a>

With live streams, few extra options are available apart from the normal functions.
As live streams are not static, a streaUrl is provided by youtube in manifest format. Here's how to grab them.

```python
>>> yt = Youtube('https://www.youtube.com/watch?v=U_XkCKlRcGQ')
>>> if yt.is_live==True:
>>> 	print(yt.hlsUrl)
>>> 	print(yt.dashUrl)
```

** This module is built for personal use. Please don't use this in production. I shall not be responsible for any consequences whatsoever.