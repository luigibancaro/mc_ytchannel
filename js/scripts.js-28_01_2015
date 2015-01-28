// JavaScript Document
//This code loads the IFrame Player API code asynchronously.
var YT_tag = document.createElement('script');
YT_tag.src = "https://www.youtube.com/iframe_api";
var firstScriptTag = document.getElementsByTagName('script')[0];
firstScriptTag.parentNode.insertBefore(YT_tag, firstScriptTag);
//This code loads the IFrame Player API code asynchronously.
var ytAPIisReady=false
var documentISready=false
var isMobileDevice=false;
var isTablet=false;
var isSmartPhone=false;
var isApple=false;

isSmartPhone=MobileEsp.DetectSmartphone()||MobileEsp.DetectIphone();
isTablet=MobileEsp.DetectTierTablet();
isApple=MobileEsp.DetectIos();
isMobileDevice=isSmartPhone||isTablet;	


function _log(data){
	console.log(data);
}

//after the API code downloads.
function onYouTubeIframeAPIReady(){ytAPIisReady=true;initGadget();}
//after document is fully loaded
jQuery(document).ready(function(e){documentISready=true;initGadget();});



function initGadget(){
	//we need both document and YT API ready for init the players
	if(documentISready&&ytAPIisReady){		
		_log('initGadget: document && ytAPI are ready');
		//var "pageType" must be defined in every pages
		if(pageType=='category'){
			allVideoPlayer.init();
		}else if(pageType=='single'){
			singleVideo.init();
		}
	}else if(documentISready){
		//for init 
		_log('initGadget: document is ready');
		gadgetHeight.init();
	}
}


var gadgetHeight={
	Element:null,
	htmlBody:null,
	height:0,
	init:function(){
		var $this=this;
		_log('gadgetHeight: init');
		$this.Element=jQuery('#the_main_gadget_wrap');
		$this.htmlBody=jQuery('html,body');
		$this.Element.imagesLoaded(function(){
			$this.resize();
			jQuery(window).resize(function(){$this.resize();});
		});
	},
	resize:function(){
		var $this=this;
		$this.height=$this.Element.innerHeight();
		$this.htmlBody.height($this.height);
		_log('gadgetHeight: resize->new_heght:'+$this.height);
		var new_height=JSON.stringify({"height": $this.height+"px"});
		top.postMessage(new_height,"https://www.youtube.com/");
		top.postMessage(new_height,"http://www.youtube.com/");
		top.postMessage(new_height,"http://localhost/");
		top.postMessage(new_height,"http://localhost/");
	}
}




var singleVideo={
	player:null,
	videoid:"",
	ratio_w4h:658/1170,
	wrapper:null,
	sharer:null,
	labels:{title:null,desc:null},
	init:function(){
		var $this=this;
		$this.wrapper=jQuery('#mc_singleVideo');
		$this.videoid=$this.wrapper.attr('data-videoid');
		$this.labels.title=jQuery('#videoTitleTarget');
		$this.labels.desc=jQuery('#videoDescTarget');
		$this.sharer=jQuery('#videoInfoTarget .mc_social_wrapper');
		$this.player=new YT.Player("videoTarget",{
			videoId:$this.videoid,
			playerVars:$this.playerVars,
			events:{
				'onReady':function(ev){
					$this.fill(ev.target.getVideoData());
				},
				'onStateChange':function(ev){
					if (ev.data==YT.PlayerState.ENDED){
						_log("singleVideo: player->ENDED");
					}else if(ev.data==YT.PlayerState.PLAYING){
						_log("singleVideo: player->PLAYING");
					}else if(ev.data==YT.PlayerState.PAUSED){
						_log("singleVideo: player->PAUSED");
					}else if(ev.data==YT.PlayerState.BUFFERING){
						_log("singleVideo: player->BUFFERING");
					}
				},
				'onError':function(event){_log(event);}
			}
		});
		$this.resize();
		jQuery(document).resize(function(){$this.resize();});
	},
	resize:function(){
		var $this=this;
		_log("singleVideo: resize");
		var video_h=$this.wrapper.width()*$this.ratio_w4h;
		_log(video_h);
		$this.wrapper.height(video_h);
	},
	fill:function(videoinfo){
		var $this=this;
		
		//fill the title
		if(typeof videoinfo.title!="undefined"&&videoinfo.title!=""){
			$this.labels.title.html(videoinfo.title);
		}else{$this.labels.title.hide();}
		
		//Sharer
		var shareUrl=encodeURIComponent('https://www.youtube.com/watch?v='+$this.videoid);
		$this.sharer.find('span.facebook a').attr('href','http://www.facebook.com/sharer.php?u='+shareUrl);
		$this.sharer.find('span.twitter a').attr('href','https://twitter.com/intent/tweet?url='+shareUrl+'&amp;text='+encodeURIComponent($this.labels.title)+'&amp;via=moncler');
		$this.sharer.find('span.gplus a').attr('href','https://plus.google.com/share?url='+shareUrl);
		/*
		http://www.facebook.com/sharer.php?u=[uri encoded URL]'
		https://twitter.com/intent/tweet?url=[uri encoded URL]&amp;text=[Title]&amp;via=moncler'
		http://tumblr.com/share?s=&v=3&t=[Title]&u=[uri encoded URL]
		https://plus.google.com/share?url=[URL]
		http://pinterest.com/pin/create/bookmarklet/?media=[MEDIA]&url=[URL]&is_video=false&description=[TITLE]
		*/
	},
	playerVars:{
		'autoplay':0,'autohide':1,'controls':1,
		'showinfo':0,'rel':0,'color':'white',
		'loop':0,'modestbranding':0,'wmode':'opaque',
		'origin':'http://'+window.location.host
	}

}
var allVideoPlayer={
	
	players:Array(),
	isInit:false,
	ratio_w4h:658/1170,
	ratio_h4w:1170/658,
	animationTime:550,
	init:function(){
		var $this=this;
		
		var $allPlayers=jQuery('.mc_videobox');

		jQuery.each($allPlayers,function(i,e){
			var an_id='video_frame_'+i;
			var $e=jQuery(e);
			var $e_frame=$e.find('.videoTarget');
			var $e_cover=$e.find('img.mc_box_image');
			var $e_overlay=$e.find('.mc_overlay_layer');
			$e_frame.attr('id',an_id);
			var $player=new Object();
			var video_player=new YT.Player(an_id,{
				videoId:$e.attr('data-videoid'),
				playerVars:$this.playerVars,
				events:{
					'onReady':function(ev){
						$player.id=an_id;
						$player.parent=$e;
						$player.cover=$e_cover;
						$player.overlay=$e_overlay;
						$player.frame=$e.find('.videoTarget');
						$player.overlay.stop(true,true).hover(
							function(){
								lock=true;
								_log("allVideoPlayer: player_"+i+"->over in");
								ev.target.playVideo();
								$player.cover.stop(true,true).animate({opacity:0},$this.animationTime);
							},
							function(){
								lock=true;
								_log("allVideoPlayer: player_"+i+"->over out");
								ev.target.pauseVideo();
								$player.cover.stop(true,true).animate({opacity:1},$this.animationTime);
							}
						);
						$this.players[i]=$player;
						allVideoPlayer.resize();
					},
					'onStateChange':function(ev){
						if (ev.data==YT.PlayerState.ENDED){
							_log("allVideoPlayer: player_"+i+"->ENDED");
						}else if(ev.data==YT.PlayerState.PLAYING){
							_log("allVideoPlayer: player_"+i+"->PLAYING");
						}else if(ev.data==YT.PlayerState.PAUSED){
							_log("allVideoPlayer: player_"+i+"->PAUSED");
						}else if(ev.data==YT.PlayerState.BUFFERING){
							_log("allVideoPlayer: player_"+i+"->BUFFERING");
						}
					},
					'onError':function(event){_log(event);}
				}
			});
			
		});
		$this.isInit=true;
		jQuery(document).resize(function(){$this.resize();});
	},
	resize:function(){
		var $this=this;
		jQuery.each($this.players,function(i,player){
			_log("allVideoPlayer: resize->player_"+i);
			var mt=0;
			var ml=0;
			var box_w=player.parent.width();
			var box_h=player.parent.height();
			var video_w=box_w;
			var video_h=box_w*$this.ratio_w4h;
			if(video_h<=box_h){
				_log('allVideoPlayer: resize->video_h<=box_h');
				video_h=box_h;
				video_w=video_h*$this.ratio_h4w;
				ml=(box_w-video_w)/2;
			}else{
				_log('allVideoPlayer: resize->video_h>box_h');
				mt=(box_h-video_h)/2;
			}
			_log('video_w:'+video_w+' video_h:'+video_h);
			player.frame.css({'width':video_w,'height':video_h,'margin-top':mt,'margin-left':ml});	
		});
		gadgetHeight.resize();
	},
	playerVars:{
		'autoplay':0,'fs':0,'autohide':1,
		'controls':0,'showinfo':0,
		'rel':0,'color':'white',
		'loop':1,'modestbranding':0,
		'wmode':'opaque','origin':'http://'+window.location.host
	}
}