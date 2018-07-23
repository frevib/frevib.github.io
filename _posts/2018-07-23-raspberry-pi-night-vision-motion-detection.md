---
layout: post
title:  "Raspberry PI with night vision camera and motion detection"
date:   2018-07-23 20:00:00 +0200
categories: raspeberry night vision motion detection
---
This tutorial will describe how to catch your house mouse with a Raspberry pi with a night vision camera and motion detection.

## Requirements ##

* Raspberry pi, any one will do. I use model 1B+
* Raspbian OS, I headless (without window manager)
* Waveshare night vision camera for Raspberry pi, with infra red LEDs
[bol.com][bol_com_camera]{:target="_blank"} 
[kiwi electronics][kiwi_electronics_camera]{:target="_blank"}


## Test the camera ##

After you've installed your night vision camera on the Raspberry pi it's time to see if the camera it working. `raspivid` is provided with the Raspbian OS to communicate with the camera. Because the PI is put in our ceiling and there is only remote access to the pi through SSH, the video output of `raspivid` needs to be streamed over the network to a device that can show the video. Let's stream the video from the PI the laptop.

On your laptop use `nc` to listen to the incoming video stream that we are going to send from the PI, and direct the stream to `mplayer`:

```bash
 nc -l 0.0.0.0 5001 | mplayer -fps 30 -cache 2048 cache-min 30 -
```

Play around with `-cache` and `-cache-min`, I had some buffer problems with `-cache 512`

Now send the video stream using `raspivid` from the PI to the laptop that is listening on port `5001`

```bash
raspivid -t 99999999 -w 1280 -h 960 -o - | nc -v 192.168.1.112  5001
```


You should see `mplayer` open and show the video stream from your PI's camera.

[pic camera mplayer]


## Motion detection ##


<!-- ekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

```javascript
const aap = geit();


```

![My helpful screenshot]({{ "/assets/hero_chara_mario_pc.png" | relative_url }})

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk]. -->


[bol_com_camera]: https://www.bol.com/nl/p/waveshare-night-vision-camera-light-sense-ir-led-board/9200000092056912/?suggestionType=featured_product&suggestedFor=wavesh&originalSearchContext=media_all&originalSection=main
[kiwi_electronics_camera]: https://www.kiwi-electronics.nl/raspberry-pi-night-vision-camera?search=waveshare 


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/