---
title: How to Build an Aircraft Band (ATC) and ADSB Receiver
date: 2022-01-17 12:00:00 -0500
categories: [Aviation]
tags: [aircraft band, atc, adsb, raspberry pi, sdr]
image_path: /assets/img/posts/how-to-build-an-atc-and-adsb-receiver/
image:
  path: /assets/img/posts/how-to-build-an-atc-and-adsb-receiver/adsb_cover.jpg
  alt: ADSB of South Western Ontario
---
Like any good project, having a goal and due date can be great motivation to "Kick the tires and light the fires".  For years, I had wanted to be able to listen to my local airport radio traffic (CYZR Sarnia, Ontario) while I was at work, or out of range of the airport.  I had looked up ADSB and SDR radios in the past but never had a good reason to buckle down and put it all together.  In 2021 Sarnia Airport had its first Save Our Airport (SOAR) fly-in to try and bring more public awareness to small regional airports and the value they continue to provide in Canada.  The fly-in was a huge success with hundreds of general aviation aircraft showing up and allowing the public to see airplanes they would not normally see.

With the 2022 SOAR fly-in announced, I decided I should get in gear and build an ADSB and Aircraft Band radio receiver so I could do a live stream of the event.

A friend of mine in Sarnia had a 35-foot tower in his backyard that already had power and network connections to it and graciously allowed me to mount my gear on it.  After doing a bunch of research on the web, this is the hardware list I came up with.

I had a couple of goals I wanted to meet with this project
- Stream ADSB data to multiple ADSB Websites
- Stream Airband Audio to a site that had a phone app so I could listen on the go

## Hardware
- Raspberry Pi 3 B+ (Has a network interface and full-size USB ports)
- 2 Nooelec NESDR SMArt SDR Radios - [Vendor Site](https://www.nooelec.com/store/sdr/sdr-receivers/nesdr-smart-sdr.html) - [Amazon.ca](https://www.amazon.ca/gp/product/B01HA642SW/ref=ppx_od_dt_b_asin_title_s01?ie=UTF8&psc=1)
  - 1 SDR was used for the live ATC Radio receiver
  - 1 SDR was used for ADSB receiver.
- Raspberry Pi Power Supply [Amazon.ca](https://www.amazon.ca/gp/product/B07CVH21NC/ref=ppx_yo_dt_b_asin_title_o05_s00?ie=UTF8&psc=1)
- 4 port powered USB Hub [Amazon.ca](https://www.amazon.ca/gp/product/B00TPMEOYM/ref=ppx_od_dt_b_asin_title_s01?ie=UTF8&psc=1)
- AirNav ADS-B 1090 MHz Outdoor Antenna [Amazon.ca](https://www.amazon.ca/gp/product/B07K7YW1XJ/ref=ppx_od_dt_b_asin_title_s00?ie=UTF8&psc=1)
- 2 Coaxial N-Type Lightning Arrestors [Amazon.ca](https://www.amazon.ca/gp/product/B07JY6TD2T/ref=ppx_od_dt_b_asin_title_s01?ie=UTF8&psc=1)
- Bunch of cable bits to connect everything
- 1 - 10 foot 3/4" Schedule 40 PVC pipe
- 1 - 3/4" Schedule 40 PVC Cap [Home Depot](https://www.homedepot.ca/product/lesso-pvc-cap-soc-3-4-inch/1000166736)
- 1 - 3/4" Schedule 40 PVC Female Adapter to Threaded [Home Depot](https://www.homedepot.ca/product/lesso-3-4-in-pvc-schedule-40-female-adapt-sxf/1000166739)
- 1 - 3/4" to 1/2" Schedule 40 PVC Bushing MxF [Home Depot](https://www.homedepot.ca/product/lesso-3-4-in-x-1-2-in-pvc-schedule-40-bushing-m-x-f/1000182036)
- 1 - 1/2" PVC Treaded strain relief fitting [Home Depot](https://www.homedepot.ca/product/thomas-betts-pvc-threaded-strain-relief-1-2-in-fitting/1000184210)
- 1 - Roll of electrical tape
- 10 feet of fishing wire
- small heat shrink tube

### Raspberry Pi
Nothing really special about the Raspberry Pi.  It did require a dedicated power supply and you will want to make sure it has enough cooling (see lessons learned below).  I attached it in the enclosure with a couple of 3M velcro Command Strips.

### Air Band Radio and Antenna
#### Antenna Calculations
I did a bunch of research on the web for what kind of antenna I would need to get an adequate range.  There were several pre-made options but in the end, I went with making my own "Flower Pot Antenna" and found that it was quite easy and worked quite well.  From listening to some of the radio chatter, this antenna has picked up transmissions from over 100 KM away which was quite understandable.  Granted, you have to have the right weather, altitude of the aircraft etc but for how easy this antenna was to make, I am very happy with how well it worked.

There were a few websites I found on the web with the original idea looking to come from [http://vk2zoi.com/](http://vk2zoi.com/) which also has a great write-up on what these are and how nice of an option this type of antenna is for urban locations.  Nomonsuhendar was another website I found [link to site](https://nomonsuhendar.blogspot.com/2020/12/flower-pot-antenna-calculator.html) that had a great calculator and I will copy all the data needed here so you can have the measurements even if those sites go down.

![https://nomonsuhendar.blogspot.com/2020/12/flower-pot-antenna-calculator.html](FLOWER_POT_ANTENNA_CALCULATOR_2.jpg)
_Parts of a Flower Pot Antenna - Credit nomonsuhendar.blogspot.com_

In North America, the spectrum used by Aircraft is 118.000 to 136.975 MHz [The RadioReference Wiki](https://wiki.radioreference.com/index.php/Aircraft).  To get the widest coverage with one antenna, my plan was to make an antenna aimed at the center of this frequency range (127.5).  Using the calculator on the Nomonsuhendar blog gives us the following numbers.  When building the Antenna, effort should be made to be as close as reasonable to these numbers but don't over-stress about getting it 100%.  I just used a regular tape measure and it was more than fine.

![https://nomonsuhendar.blogspot.com/2020/12/flower-pot-antenna-calculator.html](flower_pot_antenna_calculator_3.jpg)
_Flower Pot Antenna Calculations for Aircraft Band 127.5 Antenna - Credit nomonsuhendar.blogspot.com_

#### Building the Antenna
To build the antenna, I purchased 50 feet of N-Type Cable RG58 which was already terminated on both ends from Amazon. The cost at the time of writing this was $33.99 from [Amazon.ca](https://www.amazon.ca/gp/product/B09B79FJ2L/ref=ppx_od_dt_b_asin_title_s01?ie=UTF8&psc=1) for the cable which ensures you have a really good connector without needing to crimp your own.  I also got a 10-foot length of 3/4 inch schedule 40 PVC pipe which when cut in half is a perfect length for this antenna.  Along with the pipe, I also picked up a 3/4" end cap, 3/4" Female adapter to threaded, 3/4" to 1/2" bushing, and a 1/2" strain relief all schedule 40 PVC.  If you don't have any PVC glue, you will also need to grab a can of that.  This allowed me to build an antenna that is weathertight and should last in the elements for a long time.

To get all of the measurements right, I started by putting a long sheet of paper on my workbench and then marking out all the lengths on paper.  This was a lot easier than trying to hold the wire and a tape measure at the same time.  I also used a yellow marker to lay out all the sections on the wire before cutting anything.



### ADSB Radio and Antenna

## Software

## Build Out

## Lessons learned after running through the summer

## Links

## Calculator Code
Code copied from the calculator form on nomonsuhendar's blog.  I have not validated these calculations myself more than I followed them and found they worked well for me.

``` javascript
function calcfp()
  {
      f = document.gen2.freq.value*1 ;
      res_f = document.gen2.reson.value*1 ;
      vel_f = document.gen2.velocf.value*1 ;

      var lambda ;
      var res_freq ;
      var res_lambda ;

      lambda = Math.round(299700/( f ))/10;
      res_freq = f-(f*(res_f/100));
      res_lambda = Math.round(299700/( res_freq ))/10;

      document.gen2.lambda.value = lambda ;
      document.gen2.ur.value = Math.round(lambda/4)*0.89;
      document.gen2.lr.value = Math.round(lambda/4)*0.87;
      document.gen2.choke.value = vel_f*(res_lambda/2);
  }
```
