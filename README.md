## Youtube Descriptions Updater
Script used for updating the description of multiple youtube videos at once. 

___

## Functionality
This is a Google Apps Script (JS 1.6) that automates the proccess of updating the description of multiple Youtube videos. 

___

## Usage

1. Head over to Google's script [website](https://script.google.com/home) and sign in with the Google account that holds the Youtube videos you wish to update the description of. 

2. Create a new Google Apps Script file. 

3. Copy and paste the following code in to the code field. 
```javascript
function main() {
  var videos = YouTube.Search.list('snippet', {
    type: 'video',
    maxResults: 50,
    forMine: true
  });
  
  var allVideos = videos.items;
  
  while(videos.nextPageToken) {
    var videos = YouTube.Search.list('snippet', {
      type: 'video',
      maxResults: 50,
      forMine: true,
      pageToken: videos.nextPageToken
    });  
    
    allVideos = allVideos.concat(videos.items);
  }
  
  for(var i = 0; i < allVideos.length; i++) {
    var video = getVideo(allVideos[i].id.videoId);
    
    updateDescription(video);
  }
  
}

function getVideo(id) {
  return YouTube.Videos.list('snippet', {id: id}).items[0];
}

function updateDescription(video) {
  video.snippet.description = 'TEXT HERE \n\n' + video.snippet.description;
  
  YouTube.Videos.update(video, 'snippet');
} 
```

4. Replace "TEXT HERE" with the desired video description content.
(If you'd like to affect more or less than 50 videos, simply change "maxResults".)

5. Run the script and grant it permission to execute.

6. That's it. Your latest 50 (depending on what "maxResults" is set to) have now got their descriptions updated. 

## Disclaimer 
If you have multiple YouTube accounts connected to your Google account this will **probably** not work. Additionally, you can't hand-pick specific videos with this script. It simply updates the description of set amount of videos using it's upload date.**
