## The use of jwplayer
#### HTML code
```html
<div id='myplayer'></div>  
<input type="button" class="player-play" value="播放" /> 
<input type="button" class="player-seek" value="跳到指定位置播放" />  
<input type="button" class="player-position" value="获取播放进度" />   
<input type="button" class="player-duration" value="获取视频长度" />
<input type="button" class="player-status" value="取得状态" /> 
<input type="button" class="player-mute" value="静音" /> 
<input type="button" class="player-volume" value="设置音量" /> 
<input type="button" class="player-size" value="设置尺寸" /> 
<input type="button" class="player-full" value="全屏" />
```
#### JS code
```javascript
var thePlayer;  
thePlayer = jwplayer('myplayer').setup({
    file: 'aa.mp4',
    //file: '难念的经+_周华健.mp3',
    image: '11.jpg',
    width: '800',          
    height: '480', 
    dock: true,  
    repeat: false,//重复播放
    autostart: false,//自动播放
    controls:true,//显示控件按钮
    type: "mp4",
    //type: "mp3",
    events:{
        onComplete: function(){ console.log("播放结束!!!");},		
	onVolume: function(){ console.log("声音大小改变!!!"); },		
	onReady: function(){ console.log("准备就绪!!!"); },		
	onPlay: function(){ console.log("开始播放!!!"); $('.player-play').val("暂停");},		
	onPause: function(){ console.log("暂停!!!"); $('.player-play').val("播放")},
	onTime: function () {console.log("播放监听!!!"); /*console.log(thePlayer.getPosition());*/},
        onBufferChange: function(){ console.log("缓冲改变!!!"); /*console.log(thePlayer.getDuration());*/},
	onBufferFull: function(){ console.log("视频缓冲完成!!!");},
     }
});
  
//播放 暂停 
$('.player-play').click(function(){  
    if(thePlayer.getState() != 'PLAYING'){  
	thePlayer.play(true);  
        this.value = '暂停'; 
    }else {  
	thePlayer.play(false);  
	this.value = '播放';  
    }  
}); 
	
//跳转到指定位置播放  (需在web服务器上)
$('.player-seek').click(function() {  
    if (thePlayer.getState() != 'PLAYING') {    //若当前未播放，先启动播放器  
        thePlayer.play(true);  
    }  
    thePlayer.seek("6"); //从指定位置开始播放(单位：秒)  
}); 
  
//获取播放进度  
$('.player-position').click(function() { 
    console.log(thePlayer.getPosition()); 
});
  
//获取视频长度  
$('.player-duration').click(function() {
    console.log(thePlayer.getDuration());
});
    
//获取状态  
$('.player-status').click(function() {  
    var state = thePlayer.getState();  
    var msg;  
    switch (state) {  
        case 'BUFFERING':  
            msg = '加载中';  
            break;  
        case 'PLAYING':  
            msg = '正在播放';  
            break;  
        case 'PAUSED':  
            msg = '暂停';  
            break;  
        case 'IDLE':  
            msg = '停止';  
            break;  
    }  
    console.log(msg);  
}); 
    
//是否静音
$('.player-mute').click(function() {
    console.log(thePlayer.getMute())
    if(thePlayer.getMute()){
        thePlayer.setMute(false);
    }else{
    	thePlayer.setMute(true);
    }
});
    
//设置音量(thePlayer.getVolume()获取音量)
$('.player-volume').click(function() {
    thePlayer.setVolume(50);
});
    
//设置尺寸
$('.player-size').click(function() {
    thePlayer.resize(300, 200);
});
    
//是否全屏
$('.player-full').click(function() {
    if(thePlayer.getFullscreen()){
    	thePlayer.setFullscreen(false);
    }else{
    	thePlayer.setFullscreen(true);
    }    		
});
    
//播放中的监听函数
thePlayer.onTime(function(){
    console.log(this.getPosition());
});
