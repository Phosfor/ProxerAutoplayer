// ==UserScript==
// @name         Proxer Autoplayer
// @namespace    https://github.com/Phosfor
// @version      0.1
// @description  Lädt automatisch das nächste video
// @author       Phosfor
// @include      http://stream.proxer.me/*
// @include      https://stream.proxer.me/*
// @include      http://proxer.me/watch/*
// @include      https://proxer.me/watch/*
// @grant        none
// ==/UserScript==

function addScript(myWindow, url)
{
    var script   = myWindow.document.createElement('script');
    script.type  = 'text/javascript';
    script.src   = url;
    myWindow.document.body.appendChild(script);
}

function play()
{
    var myWindow = window.open('', 'ProxerAutoplayer', 'toolbar=0,location=0,menubar=0');
    var myVideo = $('video');
    var video;
    if(myWindow.notNew === undefined)
    {
        addScript(myWindow, 'http://code.jquery.com/jquery-2.1.4.min.js');
        addScript(myWindow, 'https://openuserjs.org/install/Desnoo/Proxer.me_HD-Video_Controls.user.js');
        
        myWindow.document.head.title = "ProxerAutoplayer";
        video = $(myWindow.document.body).append(myVideo.clone()).css("background-color","black").find('video');
        video.width('95%').height('95%');
        myWindow.notNew = true;
    }else{
        video = $(myWindow.document.body).find('video');
        video.find('source').attr('src', myVideo.find('source').attr('src'));
        video[0].load();
    }
    video.off('ended').on('ended', function(){
        window.top.postMessage('next', '*');
    });
    video[0].play();
}

var init = {
    "stream.proxer.me": function() {
        window.addEventListener('message', function(e){
            console.info('iframe recieved: ', e);
            if(e.data === 'play') {
                play();
            }
        }, false);
        
        $('#player_code').append($('<a href="javascript:;" style="background-color: black; color: white; font-family: sans-serif; font-size: 10px; opacity: 0.5; padding: 5px; position: absolute; top: 0; right:0; text-decoration:none;">Autoplay</a>').click(play));
    },
    
    "proxer.me": function(){
        window.addEventListener('message', function(e){
            
            console.info('main recieved: ', e);
            if (e.data === 'next') {
                window.location = $("a:contains('Nächste >')").attr("href")+'#autoplay';
            }
        }, false);

        if(window.location.hash === '#autoplay')
        {
            $('iframe').load(function(){
                this.contentWindow.postMessage('play', '*');
            });
        }
    }
};

$(document).ready(init[window.location.hostname]);
