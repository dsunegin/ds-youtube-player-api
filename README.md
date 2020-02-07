# It is a Fix for issue: Failed to execute 'postMessage' on 'DOMWindow': https://www.youtube.com 

According to discussion (https://stackoverflow.com/questions/27573017/failed-to-execute-postmessage-on-domwindow-https-www-youtube-com-http)
solution that works for me is:

# Solution

You can save the JavaScript into local files:

https://www.youtube.com/player_api

https://s.ytimg.com/yts/jsbin/www-widgetapi-vfluxKqfs/www-widgetapi.js

Into the first file, `player_api` put this code:

```js 
if(!window.YT)var YT={loading:0,loaded:0};if(!window.YTConfig)var YTConfig={host:"https://www.youtube.com"};YT.loading||(YT.loading=1,function(){var o=[];YT.ready=function(n){YT.loaded?n():o.push(n)},window.onYTReady=function(){YT.loaded=1;for(var n=0;n<o.length;n++)try{o[n]()}catch(i){}},YT.setConfig=function(o){for(var n in o)o.hasOwnProperty(n)&&(YTConfig[n]=o[n])}}());
```
Into the second file, find the code: `this.a.contentWindow.postMessage(a,b[c]);`
and replace it with:

```js
if(this._skiped){
    this.a.contentWindow.postMessage(a,b[c]); 
}
this._skiped = true;
````

Of course, you can concatenate into one file - will be more efficient. This is not a perfect solution, but it's works!

Source of concatenated file you can also get from  https://2-online.ru/files/public/inside/js/mediaplayer.yt_api.js

This repo contains a copy of  `mediaplayer.yt_api.js` 

# How to use 

Serve file `mediaplayer.yt_api.js` from any location.

Example: https:\\myproject.com\mediaplayer.yt_api.js

Then edit @google-web-components\google-apis\google-youtube-api.js

properties: {

    /** @private */
    libraryUrl:  {
      type: String,
      value: 'https:\\myproject.com\mediaplayer.yt_api.js'
    },

That's all

## Contribute
Feel free to extend it or propose new functionality

## License

[MIT License](https://github.com/dsunegin/ds-youtube-player-api/LICENSE)
