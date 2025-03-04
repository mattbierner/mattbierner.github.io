---
layout: post
title: "I Am A Camera"
date: '2017-04-18'
description: "Hooking a Minolta SR-T 101 up to a VR headset and passing into glass reality"
series: modded_reality
titleImage:
    file: "title2.jpg"
---

{% include image.html file="title2.jpg" %}

Oh to be a camera! To see light! To live through a lens! To record your memories! If only, if only...

*I Am A Camera* explores a very literal take on this idea by streaming video from an old Minolta SR-T 101 film camera to a VR headset. The resulting view more or less matches what would be captured on film.

This experiment is part of a series that I've termed *[modded reality][mr]*, which explores using technology to remix and alter one's sensory experience of the world. This camera based device certainly fits that description, most significantly by allowing you to manually control the focus and depth of field of your sight. It transforms the camera from a bland recording tool into a device that changes your experience of the world. While wearing the headset, you can just glimpse the unique take on reality that our imperfect technology comprehends. Also, it makes looking like a fool more fun than ever before.

This post covers the entire experiment: from concept, to implementation, to experience. Let's take a look.


# Camera
As a lover of photography and of classic camera gear, my interpretation of *I Am A Camera* was driven by how I normally shoot. I've shot 35mm film on an old pawnshop SLR and I've shot digital with the latest lenses—and I've surely shot plenty with my phone too—but the gear pairing that I keep returning to is a modern digital camera with a selection of fully manual lenses. This mix of analog control and digital capture is what I enjoy using; the process of manually focusing and dialing in the camera makes me slow down and consider scenes differently. It's also just more rewarding to shoot with somehow.

Which brings up another important point: *I Am A Camera* had to be just as much about the photography experience as it was about the gear itself. Merely looking through a lens would not do; I wanted to capture both the physical act of operating a camera and the less tangible social and sensory aspects of photography.

For these reasons, I decided to use a classic film camera body as the base for my new device, and upgrade it with a digital sensor. This would provide a nice starting point and also capture an authentic photography experience. Besides, the absurdity of hooking all this modern technology up to a big old film camera system is very much in the spirit of *modded reality*.

But which camera to use? It had to be an older model, one that was mostly manual in operation, but also not so ancient that the hardware or shooting experience would be unfamiliar to a modern user. And really, for me there was only ever one correct answer: Minolta's timeless SR-T 101. This is the quintessential film SLR, and it remains instantly recognizable and understandable even fifty years after its release.

{% include image.html file="srt101.jpg" %}

As ’twould be a shame to destroy any old camera—and since destruction was a near certainty in my ever capable hands—I opted to use a SR-T 101 that was already well and truly broken: the electronics didn't work, the film door wouldn't shut, and the shutter was torn. That being said, this particular *Goodwill* castoff did have some magic left in it, magic in the form of a unicorn sticker boldly affixed to the prism housing. Amazing.

{% include image.html file="unicorn.jpg" description="Fuck the red dot; unicorn forever!" %}

Now it must be said that if you're trying to replicate a modern photography experience, rather than a big old SLR, a far more practical and far more relevant setup would be to capture images through a phone. That's fine, but that's also not what I was interested in here. No, I was after the more manual and tactile feel that an older camera system provides. Not to mention, the SR-T 101 is just as much a prop for the social commentary aspects of this experiment, as it is a practical device for seeing the world.

# Attempt One: Digital Back
With the camera chosen, my next task was to find a way to capture video that more or less matched what the camera would normally record to film. I wanted to capture the unique views offered by mounting any of Minolta's old Rokkor lenses on the SR-T 101, as well as image effects such as focus and depth of field.

My first thought was to create a poor-man's digital-back by swapping out the film for a digital sensor. While this sounded great in theory, realizing it on the cheapproved problematic.

To begin, I stripped down an [ELP fisheye USB webcam][elp] to remove its built-in optics and other such accoutrements, leaving the sensor directly exposed. I then mounted the sensor roughly where the film would go in the SR-T 101.

{% include image.html file="back-setup.jpg" %}

I then connected the webcam sensor to a Raspberry Pi 2 using USB. For portability, the Pi was powered by a small phone battery, with the computer + battery pack most conveniently stored in a pants pocket.

The software pipeline was identical to what [I've covered previously][tenome]. The Raspberry Pi creates an mjpeg video stream from the USB camera and sends it to an iPhone mounted inside of a Google Cardboard headset over a personal hotspot network. The software is simple to setup and has worked well enough for previous experiments.

Somehow this crude setup actually works. It more than just works actually; the resulting images are fairly sharp and clear. Not bad for something thrown together in a few minutes and held together by rubber bands and tape.

{% include image.html file="back-image.jpg" description="Sample image of the above setup using a 50mm lens. Not bad, but something about the scale seems a bit fishy..." %}

Yet when I mounted a lovely 50mm Rokkor on the camera and loaded up the video stream for the first time, a fatal flaw of this setup quickly became apparent; rather than a normal 50mm view, the image on my screen was closer to something taken with a 300mm telephoto. Not only does this differ from what the camera would normally capture, but a 300mm telephoto is not the most practical viewport for navigating about the world. Not that I didn't give telephoto reality the old college try of course. Being somewhat of a connoisseur of shit reality—sometimes in a far too literal sense of that term—I found experiencing the world in telephoto to be intriguing, albeit highly disorienting. You really need a tripod to hold your eyes steady here, and the impact of sudden camera movements is not to be underestimated. 

However, telephoto reality was not what I was going for when I conceived of this digital-back. With a 50mm lens, I expected a 50mm field of view. What happened?

Well, the sensor was mounted roughly where the film would normally sit in the camera, which explains why the image itself was fairly clear and sharp, and why focusing more or less worked as expected; light travels the same distances from the lens to the sensor. The difference is area. The ELP webcam sensor measures a hearty 5.8 x 3.7mm, for an area of around 21.5mm². Not bad, not bad. But consider this: normal 35mm film clocks in at 36 x 24mm for an area of around 864mm², some 41 times the webcam's. That means that the webcam's sensor can capture only a fraction of the whole 35mm image.

{% include image.html file="back-film-size.svg" %}

Comparing the diagonals of the two sensors, we see that the crop factor of the webcam is some 6.3. This stretches a 28mm wide-angle into a 175mm telephoto, and turns a 50mm normal lens into a 315mm monster. *Damn!*

{% include image.html file="back-sensors.jpg" description="Difference in sensor size with an APS-C NEX 7" %}

For fun, I also tried mounting a real 300mm lens on the camera to create a well-nigh Galilean 1900mm. That my crude setup produced recognizable images at all really speaks more to the quality of Minolta's old glass than anything else; after all it's just a cheap webcam rubberbanded into the camera's film bay for Christ's sake.

Although plenty fun, a proper digital-back would require a sensor roughly the same dimensions as 35mm film. Either that or additional optics to adjust the image to the webcam sensor's much smaller size. Both of these had a lot of unknowns, so I decided to try a completely different approach instead.


# Attempt Two: Camera Mount
But come now, didn't the usb webcam already include some perfectly good optics? Surely we can avoid all the magnification trouble by just mounting the webcam on the SR-T 101 like a proper old Rokkor! Good idea.

If the new webcam lens was to be worthy of its Rokkor forefathers, tape and rubber bands clearly would never do. So I designed and printed a simple ELP usb webcam to Minolta adapter. The back of the mount locks into the camera bayonet-style. The webcam itself is sandwiched between the back plate of the mount and a front cover plate.

{% include image.html file="lens-mount-front.jpg" %}
{% include image.html file="lens-mount-back.jpg" %}

When on the camera, the usb webcam is correctly horizontally aligned for landscape shooting. Again, while prototyping here, I simply ran the usb cable through the back of the SR-T 101 to my pocketside Pi. The mount worked far better than expected, and the image from the USB webcam was just what I was hoping for: clear, clean, and—unlike in the prior attempt—the field of view was quite wide.

After putting the new device through its paces however, I walked away less than impressed. There's really no nice way to say it: it's just dull.

Understand: although not even a year has passed since I began my exploration of modded reality, quite a bit of ground has been covered in that time. So after having experienced reality through cameras mounted on my hands, or on my feet, or well-nigh everywhere in between—and having stuck cameras to selfie sticks, and having explored mixing in some pharmacology, and having peered out of various orifices at the world, and having lived in the third person, and jived to the vibrations of the universe—just affixing a webcam to an old camera hardly qualifies as exciting anymore. (The rapid build up of tolerance here does not bode well for the future.)

{% include image.html file="lens-mount-usage.jpg" description="Tourist of the future" %}

The best parallel for the device here is to my [hand-for-eyes experiment][tenome], but even compared to that initial foray, this setup felt bland. Using the device, I instinctively assumed the same stance I would when carrying my normal camera, holding the camera up near the center of my chest with one arm. This provided a far more stable view than the camera strapped to my hands ever did, and also eliminated many spontaneous movements. And because I was holding onto the camera, I never fell into the previously all too common trap of trying to reach out and open a door with one of my eyes.

Furthermore, because only a single camera was used in this setup, the image quality was actually fairly good, with none of the ghosting or other weird effects that occurred when I tried to view separate video streams with each eye.

So all-in-all, you could say this camera mounted eye is a real step-up from those crude *Pale Man* days, what with their nausea, disorientation, and infinite awkwardness. I must disagree however, for to live is not to feast only upon the finest fair, and if we sometimes find *Queen Divine's* preferred dish far more flavorful than the finest foie gras, so be it. And who's to say we're wrong, for, at the end of the day, it's all just shit.

So no, this boring old setup clearly would never do. 


# Attempt Three: Viewfinder
Seeking a more novel experience, it struck me that I really had been making this all a whole lot more complicated than it needed to be. Looking through a camera already is a solved problem—the viewfinder shows our eyes what the captured image will look like—so why not just use that? Duh.

To look through the camera's viewfinder, I swapped the fisheye webcam for a Raspberry Pi Camera Module V2. This unit is much smaller and has a much more appropriate 30mm equivalent focal length.

{% include image.html file="device-viewfinder3.jpg" %}
{% include image.html file="device-viewfinder1.jpg" %}

To hold the camera module in place, I designed and printed a simple mount that slides into the grooves on the side of the SR-T 101's viewfinder. This mount positions the Raspberry Pi camera about 15cm from the viewfinder's glass. I did try slightly adjusting the lens on the camera module to focus more closely, but it's still not great. And it's not like the optics of the camera's viewfinder are great either. Consider the path that light is taking here: it comes into the camera through a 50 year old lens, bounces off a somewhat dubious mirror, streams up through a prism, then through the optics of the viewfinder, onto the cheapo optics of the Pi camera module, and finally onto the digital sensor itself. It's actually a minor miracle that the thing works at all.

But *work* it does! And better yet, this configuration provides an image that approximates what the camera would capture on film. The field of view is a bit narrower than normal, and the image quality is terrible in a way that only a filter loving Instagramer could appreciate, but this all actually mirrors the experience of shooting with old gear. The dust and debris and haziness all just add to the authenticity. (File under: "Things a hipster says".)

# Completing the Setup
Having devoted so much time to optics, let us review the rest of the device at a more spirited pace.

The Pi camera module is connected to a Raspberry Pi Zero V3, housed in a small 3D printed case affixed to the back plate of the camera's film door. This unfortunately prevents the camera from closing properly, even after I removed many of the camera's internal components.

{% include image.html file="device-battery2.jpg" %}

The Pi is powered by a 3250mh lipstick style phone battery, contained in a custom battery-grip style housing. This whole mess attaches to the camera using two bars that lock over the camera body.

{% include image.html file="device-iso.jpg" %}

{% include image.html file="device-top.jpg" %}

The design here is hardly elegant. When it comes to hardware, I'm the most amateur of amateurs, and cosmetics are never a priority. And while I have little doubt that someone with more skill could fit the entire setup cleanly inside a SR-T 101 body, I do somewhat enjoy the hacked together aesthetic of the current unit.

For streaming video from the Pi to the iPhone, the basic software pipeline remains more or less the same as my original [hands-for-eyes experiment][tenome]: video from the camera is shared using mjpeg-streamer over LAN with the iPhone. This time around though, I wanted to experiment further with wireless streaming.

Most of my previous experiments transmitted video from the Pi to the iPhone over a lightning cable, which provides a consistent, high-bandwidth, low-latency stream. A wired setup is hardly ideal however. For one, when the cables come disconnected, it freezes the video stream and leaves you blind. Naturally, this always seems to happen at the most inopportune of times. Furthermore, wires limit what is possible: they stop you from moving freely or from mounting cameras in all sorts of fun potential locations. Not to mention, while becoming intwined with your partner in a cyberpunk rat-king may be funny the first one or two times it happens, the novelty quickly wears thin.

My wireless streaming approach again makes use of the iPhone's personal hotspot feature, and getting started was incredibly simple: I just configured `wpa_supplicant` on the Pi to connect to my iPhone's personal hotspot. Once the Pi connects, it can then be accessed from any other device on the hotspot by IP address, which will fall somewhere in the range: `172.20.10.x`. Using the IP, I could ssh into the Pi from my phone to initiate streaming, as well as connect to the mjpeg stream itself. (I wasn't able to get hostname resolution working properly however, so I always had to use the IP.)

{% include image.html file="tripod1.jpg" description="While I forgot to add a tripod mounting screw hole, it this was nothing a little gaffer tape couldn't fix" %}

For all the usability benefits of wireless streaming, there are some pretty major downsides, not the least of which is latency. Even with a very low resolution stream, the lowest glass-to-glass latency I saw using wireless streaming on the personal hotspot was somewhere around 200ms, roughly double that of the wired connection. The latency also tends to be spiky. Increasing the resolution or framerate of the stream only makes these problems worse. 720p at 30fps proved to be the limit of acceptable performance, and I chose to go with a 640x480 stream at 48fps for reliability. Sure the resolution sucks, but the latency is consistently lower.

Finally, to complete the photoshoot experience, I hooked up the SR-T 101's shutter button to the Pi's GPIO. Depress the button and a python script uses OpenCV to capture the current frame of the stream and post it to Twitter under the *Buggles* inspired `#iamacamera` hashtag. (Yes they actually did record more than one song.) Instagram or SnapChat would be a better fit for this type of project, but those crusty old fucks don't provide an official API for uploading images, and my non-sanctioned API usage attempts on those platforms have never ended well. 

And that's the hardware. As you can see, it's all just cheap consumer grade stuff duct taped together, in both a metaphorical and literal sense. Nothing fancy. But this little device actually turned out to be by far the most robust modded reality gadget that I've put together thus far. With my phone no longer drawing power from the Pi, I could get well over three hours of streaming, and with no cables to jostle loose, the wireless stream was surprisingly durable. My damn phone's battery was actually the limiting factor here, and I had to use another portable battery pack just to keep it limping along. 


# A Day at the Park
With the device complete, I sallied forth into the wide world, camera in hand, just as I had countless times before. But this time was different, for no longer would I just be photographing the world with my camera, I would be experiencing the world *as* my camera. My destination: [Olympic Sculpture Park](http://www.seattleartmuseum.org/visit/olympic-sculpture-park), which I figured would provide some interesting sights, as well as a nice backdrop for filming the experiment itself.

I began over by Richard Serra's *Wake*, a series of five rusted, wave-like hulks that dominate one side of the park. While unpacking the perhaps somewhat suspicious appearing setup, I could sense a bit of side-eye from the nearest security guard. Strapping a cardboard box to my face probably didn't help matters, but after that I could no longer see well enough to say for sure.

{% include image.html file="shot3.jpg" href="https://twitter.com/mattbierner/status/843559862642532352" %}

Observing my surroundings for the first time, what I immediately noticed was the image quality. It was not good. Not that this was unexpected of course. The SR-T 101's viewfinder was never intended for capturing images; it provides users a rough but workable preview of what the image will look like on film. Now try viewing that merely workable preview using a tiny cheap digital sensor—one that was in all likelihood entirely improperly focused—and down-sample the result to a 640x480 jpeg. The result is kind of a blurry mess, especially when used for VR. Don't get me wrong though; once I adapted, I actually came to appreciate the low quality because it gives the view a truly authentic classic camera visual style. No filters needed here.

I started the shoot with 28mm MD Rokkor f2.8 lens. Normally a 28mm lens would be considered a wide-angle, but used for my vision, the view actually felt fairly claustrophobic. This made me really appreciate just how wide normal human vision is. Using this lens, I could only really see objects directly in front of me, not those to the side or even those on the ground in front of me. I had to pan the camera about to get any real sense of my surroundings.

{% include youtube.html width="560" height="315" src="https://www.youtube.com/embed/ib_I_BT2iCY" description="Video from the SR-T 101 is a little more choppy than it was in person. It was not smooth there either however" %}

My first movements while wearing the device were highly tentative. Many of the challenges here were similar to those that I faced when [using my hands as my eyes][tenome]. The narrower field of view left me constantly feeling that I was about to walk into a wall or stumble over something. 

It was difficult to even place myself within my surroundings. There was no longer the same relation between what I was seeing and the orientation of my head or my body. With my head facing forward, my brain usually interpreted what I was seeing to be what was in front of me. Only when I tried stepping forward would I realize that the camera had actually been misaligned 30 degrees or more from my body. My hopelessly outmoded brain had been fooled into thinking that my head was located where the camera was. To counteract this, I had to consciously integrate with where I was holding the camera and my sense of my body's orientation.

{% include image.html file="selfie.jpg" description="Selfies are a key to being a camera" %}

I actually adapted to this new reality much more quickly than I did to having my eyes on my hands however, at least partially because I naturally kept the camera close to my body near the center of my chest. This fixed position made the experience closer to the chest mounted camera setup that I experimented with. But I had only ever toyed around the hand cameras and body cam setups the relative safety of my apartment; out in the wide world, walking the rusted passages of this *Berzerk* style maze, I had to be much more cautious, lest I bowl over some small humanoid or—even worse—bump into the artwork.

While getting used to the reality, I also began the photoshoot in ernest. This proved to be an interesting mix between live-streaming and traditional social media photo-sharing. When I pressed the shutter button, there was no chance for editing or reviewing or even confirming before the image went public—the image went directly from my eyes up to the internet. Unlike live-streaming however, the camera still allowed me to construct and present specific images instead of capturing every moment.

{% include image.html file="shot1.jpg" href="https://twitter.com/mattbierner/status/843567666614874112" description="Ooh, so artsy! Should have shot in black and white though" %}

You can find the [complete set of images on Twitter][twitter]. With the exception of one or two prototype shots, all of the stills were captured by me as a camera with no post-processing or editing. The camera lacked orientation detection, so some of the shots may be off. Looking over the resulting images, I feel that the scenes looked far better in my head then they do on film, but a few shots are interesting. And that's pretty much par for the course when it comes to photography anyways.

As I snapped away, I even imagined that my little experiment must be pretty cutting edge. "It's like I'm living in the future!" I most cocksurely thought to myself, "I must be the first human to ever experience the world as a camera." So just imagine how crushing it was when I took off my goggles and observed countless small groups of tourists dutifully marching from sculpture to sculpture, phones in hand, eager to capture the day's next sight. After that, camera reality seemed pretty mundane.

On that note, I must mention this device's performance art potential. This experiment does unfortunately fit that bill, what with its bizarre public behavior and totally not subtle edge of social commentary. However, not wishing to become another Seattle *Surveillance Camera Man*, rather than seek out the spotlight, if anything, I sought out more isolated places for my photographic adventures.


# The Joys and Perils of Life in 50mm
Having successfully navigated *Wake*, I now upped the challenge by switching to a 50mm lens and moving to a small gravel path that tours past several smaller sculptures. As would be expected, the 50mm lens provided a considerably narrower field of view compared to the 28mm, and given that I previously found even 28mm vision somewhat constricted, you can imagine the problems this presented. Try holding a 3x5 index card about eight inches from your eyes to get some idea of the viewport here.

{% include youtube.html width="560" height="315" src="https://www.youtube.com/embed/v8jBzDpztWU" description="There is nothing wrong with your television set. Do not attempt to adjust the picture..." %}

The narrower view only amplified all of the problems that I previously described. The experience was even more claustrophobic; I could never get a good sense of what was going on around me or if I was about to walk into some innocent bystander. Even walking along the gravel trail in a straight line proved almost impossible, and, not wanting to trample any flowers, I had to constantly aim the camera at my feet so that I could adjust my course. Sobriety testing would not have gone well.

But I don't want to sell 50mm reality short. There are rewards to be found here if you can look beyond the disorientation and nausea, not the least of which is having full manual control over your eye's focus. Think of it: in regular old reality, your eyes focus more or less automatically on whatever you look at, be it six inches away or six hundred feet. It is true that, with concentration, some modicum of control is possible, but this hardly measures up to what a camera offers.

The camera decouples focusing from what you see, it allows you to slide the plane of focus through a scene or render the world in a beautiful bokeh. Having to manually manage the focusing of your eyes is an additional challenge to be sure, but one that is too unique to pass up.

{% include youtube.html width="560" height="315" src="https://www.youtube.com/embed/5-1wbkDdNnk" %}

The most pronounced effects came using the 50mm lens to focus on objects closer than three feet or so away. At large apertures, when focused on a foreground object, the background was a wonderful blur of bokeh. I could even study the out of focus sections in depth, something that is very difficult to do with normal vision. Exploring some flowering trees was also neat, as their complex three dimensional shapes offered a range of focusing possibilities.

I must also admit that walking around with my eyes entirely out of focus was entertaining. In this configuration, the world became a disorienting mess of colors and shapes, as if wearing a bad pair of prescription lenses. Practical: no; fun: almost definitely.


# Further Exploration
Having proven myself utterly incapable of even walking a straight line with 50mm reality, I switched back to 28mm reality to continue my exploration of the park.

{% include youtube.html width="560" height="315" src="https://www.youtube.com/embed/P92ROY1Jy4w" %}

A brief stop atop a pedestrian bridge offered me a good view of downtown Seattle. This is where the low image quality of the stream was most noticeable. The downtown waterfront scene before me had tons of detail, yet I could make out almost none of it.

This is also a good point to mention aperture control and the Raspberry Pi camera's automatic adjustment settings. Many of the scenes at the park had significant amounts of contrast, with the ground cast in shadow while the sky was a brilliant blue. Capturing such a scene on a old film camera would normally require good knowledge of light and exposure, with the photographer adjusting the camera based on lighting conditions. My time as a camera was made considerably easier however by the Raspberry Pi camera's automatic adjustment settings. This put the camera into something akin to aperture priority mode. Looking up at a tree branch in the sky for example decreased the exposure time of each frame drastically, while staring into the shadows caused the exposure to go up again.

This automatic control meant one less thing to worry about but, in retrospect, I'm not entirely happy with this crutch. I think it would have been far more interesting to have the camera operate in full manual shooting mode, even if that would have meant plenty of overly bright or dark scenes.

As I grew somewhat more comfortable moving about in camera reality, I began to focus more on seeking out and capturing appealing pictures. It actually started to feel similar to a normal photoshoot, although perhaps amplified or intensified in some way.

{% include youtube.html width="560" height="315" src="https://www.youtube.com/embed/7GWQbzcih7I" %}

{% include youtube.html width="560" height="315" src="https://www.youtube.com/embed/DwuDb2wLtxY" %}


Photography for me has always been more about the shooting experience than the resulting images. With my camera, I see the world differently, or rather my camera gives me an excuse and reason to explore the world visually. Out on a photoshoot, I don't see my surroundings as what they are but as the potential scenes and shots they may contain. That's how I began to feel while looking through this camera.

The entire experience was very surreal as well. I felt oddly detached from my body and my surroundings, more like an observer walking an unfamiliar world and discovering its beauty. All in a good way though. The real difference from a normal photoshoot is that I could never put the camera down or stop seeing in this way—besides just taking off the device of course—and the longer I used the device, the stronger the effect became, until normal reality began to feel like the odd bit. For me at least, the device really did capture many of the intangible qualities that I most enjoy about photography.

In my normal photography, I often find what may be considered unremarkable or even ugly scenes far more visually interesting than the more standard subjects on offer at a place like *Olympic Sculpture Park*. So later in the day, I made a passing attempt at shooting one of my more traditional photographic muses: namely, a parking lot.

{% include youtube.html width="560" height="315" src="https://www.youtube.com/embed/6TBJF8vSoK8" description="There by the waterside..." %}

Outside the relative safety of the park, experiencing the world as a camera is considerably more stressful. Standing in my lovely parking lot studio, my narrow field of view left me constantly in fear that a car would come zipping about, bringing with it the potential for many a *Darwin Award*. 

And by now, my phone was limping along, having already drained its own battery and most of a second portable battery pack. Besides, I'd seen what I wanted to. So off with the goggles. Back to base reality.


# Thoughts
{% include image.html file="title3.jpg" %}

On one hand, a camera quantifies the world, reduces its enumerable complexities and subtleties and beauties down to a mass of silver specks on film or a series of bytes on a chip. But through this reduction, we gain a new way of seeing and new form of expression, and it's this that fascinates me most about technology more broadly. I put myself into the camera not to reduce myself to it but to broaden my sight.

Looking through the modified SR-T 101, it didn't matter that the video was low quality, shaky, and blurry. No, the most fun came from embracing these limitations, from experiencing the world with bokeh and depth of field and other effects unique to the medium.

And after getting over the initial disorientation, I began to appreciate how this device amplified what I enjoy most about photography. It changed how I saw and experienced the world, turning an ordinary stroll through the park into something far more memorable. It truly was an adventure in modern reality.


[tenome]: /tenome
[mr]: /series/modded_reality

[elp]: https://www.amazon.com/180degree-Fisheye-Camera-usb-Android-Windows/dp/B00LQ854AG

[twitter]: https://twitter.com/search?src=typd&q=%23iamacamera%20from%3Amattbierner



