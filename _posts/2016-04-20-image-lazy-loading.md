---
layout: post
lang: jQuery
title: Image Lazy Loading
comments: true
description : Loading images based on scroll position
date: 2016-04-20
header_desc: jQuery - Image Lazy Loading
---
<p>Lazy load images based on scroll position</p>

```html

<!DOCTYPE html>
<html>
	<head>
	<meta http-equiv="content-type" content="text/html; charset=UTF-8">
	<title>Flickr lazyloading</title> 
        <link rel="stylesheet" type="text/css" href="http://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">  
        <style>
	.flickr .imgWrap {
		border: 3px outset #666;
		line-height: 140px;
		margin-bottom: 10px;
		margin-top: 10px;
		-moz-box-shadow: 2px 2px 5px #333;
		-webkit-box-shadow: 2px 2px 5px #333;
		box-shadow: 2px 2px 5px #333;
		width: 300px;
		height: 200px;
	}
	.flickr .imgWrap img {
		width: 300px;
		height: 200px;
	}
	.flickr .imgWrap img.lazy {
		display: block;
		width: 100px;
		height: 100px;				
		margin-left: auto;
		margin-right: auto;
		margin-top: 15%;
	}
	</style>
	<script type="text/javascript" src="http://code.jquery.com/jquery-1.11.0.js"></script>  
  
	<script type="text/javascript">
        function isVisible($element)
        {
	        // Get the current page visible area in the user screen
	        var topView = $(window).scrollTop();
	        var botView = topView + $(window).height();
	        var topElement = $element.offset().top;
	        var botElement = topElement + $element.height();				
	        return ((botElement <= botView) && (topElement >= topView));
        }
        function lazyLoadImage(){
	        $('.flickr img.lazy').each(function(){
		        if (isVisible($(this).closest('.imgWrap'))) {				
			        $(this).one("load", function() {
				        $(this).removeClass('lazy');
			        }).attr('src', $(this).attr('data-src')).removeAttr('data-src');						
		        }
	        });
        };
        $(function () {
        
        	$(window).scroll(lazyLoadImage);
        
        	var imageList='';
        				
        	$.getJSON('http://api.flickr.com/services/feeds/photos_public.gne?tags=nature&tagmode=any&format=json&jsoncallback=?', function(data) {
                    $(data.items).each(function(i, item) {
        	            imageList+='<div class="col-md-4"><figure><div class="imgWrap"><img class="lazy" src="ajax-loader_b.gif" data-src="'+item.media.m+'"/></div><figcaption>'+item.title+'</figcaption></figure></div>';						
                    });
        					
                    if (imageList=='') {
        	            $(".flickr").html("Error while loading images");
        	            return
                    }
        					
                    $(".flickr").append(imageList);
        						
        		var $images = $(".flickr .col-md-4");
        		for(var i = 0; i < $images.length; i+=3) {
        			$images.slice(i, i+3).wrapAll("<div class='row'></div>");
        		}					
        		   
        		lazyLoadImage();
        	});
        			 
        });
	</script>
	</head>
	<body>
		<div class="flickr">
		</div>
	</body>
</html>

(function ($) {
  function isVisible($element) {
      var topView = $(window).scrollTop();
      var botView = topView + $(window).height();
      var topElement = $element.offset().top;
      var botElement = topElement + $element.height();
      return (topElement + 100 <= botView);
  }

  function lazyLoadImage() {
      $('.lazy-loading').each(function () {
          if (isVisible($(this))) {
              $(this).one("load", function () {
                  $(this).removeClass('lazy-loading');
              }).fadeOut(400).attr('src', $(this).attr('data-url')).removeAttr('data-url').fadeIn(400);
          }
      });
  }

  $(window).scroll(lazyLoadImage);

}(jQuery));


// My Custom Version

(function ($) {
  function isVisible($element) {
      var topView = $(window).scrollTop();
      var botView = topView + $(window).height();
      var topElement = $element.offset().top;
      var botElement = topElement + $element.height();
      return (topElement + 100 <= botView);
  }

  function lazyLoadImage() {
      $('.lazy-loading').each(function () {
          if (isVisible($(this))) {
              $(this).one("load", function () {
                  $(this).removeClass('lazy-loading');
              }).attr('src', $(this).attr('data-url')).removeAttr('data-url height width').fadeIn(400);
          }
      });
  }

  //Load if images already in viewport
  lazyLoadImage();

  //Load on Scroll
  $(window).scroll(lazyLoadImage);

}(jQuery));

```
