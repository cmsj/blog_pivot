Title: Raspberry Pi project: PiBus
Date: 2015-12-08
Tags: FOSS rPi Python

The premise of this project is simple - my family lives in London, there's a bus route that runs along our road, and we use it a lot to take the kids places.
The reality of using the bus is a little more complex - getting three kids ready to go out, is a nightmare and it's not made any easier by checking a travel app on your phone to see how close the bus is.

I don't think there is much I can do to make the preparation of the children any less manic, but I can certainly do something about the visibility of information about buses, mainly thanks to the excellent open APIs/data provided by [Transport For London](https://tfl.gov.uk/info-for/open-data-users/).
So, armed with their API, a Python 3 interpreter and a [Raspberry Pi](https://www.raspberrypi.org/), I set out to make a little box for the kitchen wall which will show when the next 3 buses are due to arrive outside our house.
[The code itself](https://github.com/cmsj/pibus) is easy enough to throw together because Python has libraries for everything (it also helps if you don't bother to design a decent architecture!). [Requests](http://docs.python-requests.org/en/latest/) to fetch the bus data from TfL, [json](https://docs.python.org/3/library/json.html)/[iso8601](https://pypi.python.org/pypi/iso8601) to parse the data, [Pillow](https://python-pillow.github.io/) to render it as an image, and [APScheduler](https://apscheduler.readthedocs.org/en/latest/) to give it a simple run-loop.
The question then becomes, how to display the data. The easiest answer would be a little LCD screen, but that brings with it the downside of having a backlight in the kitchen, which would be ugly and distracting, and it also raises the question of viewing angles. Another answer would be some kind of physical indicator, but that requires skills I don't have time for. Instead, I decided to look for an E-Ink display (think [Kindle](https://kindle.amazon.com/)) - it would let me display simple images without producing light.
The first option I looked at was the [PaPiRus](https://www.kickstarter.com/projects/pisupply/papirus-the-epaper-screen-hat-for-your-raspberry-p), but it's in the window between its crowdfunding drive having finished, and being available to buy. The only other option I could find was the [E-Paper HAT](http://www.percheron-electronics.uk/shop/e-paper-hat/), from Percheron Electronics, which also started life as a crowdfunding project, but is actually available to buy.
Unfortunately, these displays are super fragile, which I discovered by destroying the first one, but Neil at Percheron was super helpful and I quickly had a new display and some tips about how to avoid cracking it.
My visualisation of this data isn't going to win any awards for beauty, but it serves its purpose by showing a big number to tell us how many minutes we have, and I managed to minimise the number of times you see the white-black-white refresh cycle of the eInk display with partial screen updates.
Here are some photos of the project in various stages of construction:
|                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<img src="http://2.bp.blogspot.com/-QxE9lMNZ0LA/Vmbd-ZdiM-I/AAAAAAAABJ0/rWJby-17kg8/s320/IMG_6856.JPG" width="320" height="240" />](http://2.bp.blogspot.com/-QxE9lMNZ0LA/Vmbd-ZdiM-I/AAAAAAAABJ0/rWJby-17kg8/s1600/IMG_6856.JPG) |
| Freshly assembled out of the box                                                                                                                                                                                                   |

|                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<img src="http://3.bp.blogspot.com/-FMNJfilcQiA/Vmbd-U_9LHI/AAAAAAAABJ8/aPX8QQyNbUA/s320/IMG_6857.JPG" width="320" height="240" />](http://3.bp.blogspot.com/-FMNJfilcQiA/Vmbd-U_9LHI/AAAAAAAABJ8/aPX8QQyNbUA/s1600/IMG_6857.JPG) |
| The smallest USB WiFi adapter I've ever seen                                                                                                                                                                                       |

<table>
<tbody>
<tr class="odd">
<td><a href="http://4.bp.blogspot.com/-JFevEkKVfWY/Vmbd-ZyLRHI/AAAAAAAABJ4/6zIdFnowxBQ/s1600/IMG_6858.JPG"><img src="http://4.bp.blogspot.com/-JFevEkKVfWY/Vmbd-ZyLRHI/AAAAAAAABJ4/6zIdFnowxBQ/s320/IMG_6858.JPG" width="320" height="240" /></a></td>
</tr>
<tr class="even">
<td>Sadly I had to make some modifications to the<br />
PiBow case to fit this particular rPi</td>
</tr>
</tbody>
</table>

|                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<img src="http://2.bp.blogspot.com/-TlzN7f0WSo4/Vmbd_slt11I/AAAAAAAABKM/wH_v99x5zOI/s320/IMG_6864.JPG" width="320" height="240" />](http://2.bp.blogspot.com/-TlzN7f0WSo4/Vmbd_slt11I/AAAAAAAABKM/wH_v99x5zOI/s1600/IMG_6864.JPG) |
| Running one of the eInk display test programs                                                                                                                                                                                      |

 Initially I was rather hoping I could use the famous font that TfL (and London Transport before it) use, which is known as Johnston, but sadly they will not licence the font outside their own use and use by contracted partners. There is a third party clone of the font, but it may have legal issues, presumably because TfL values their braaaaaand. Instead, I decided to just drop the idea of shipping a font with the code, and exported Courier.ttf from my laptop to the Pi directly.

|                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<img src="http://1.bp.blogspot.com/-gMsmRf-nciA/Vmbd_5_QMsI/AAAAAAAABKQ/x998LGpavUA/s320/IMG_6897.JPG" width="320" height="240" />](http://1.bp.blogspot.com/-gMsmRf-nciA/Vmbd_5_QMsI/AAAAAAAABKQ/x998LGpavUA/s1600/IMG_6897.JPG) |
| This would have been nice, but I cannot have nice font things.                                                                                                                                                                     |

I did briefly try Ubuntu Mono, which is a lovely font, but the zeros look like eyes and it freaked me out.

|                                                                                                                                                                                                                                                       |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [![](http://1.bp.blogspot.com/-Lo25lMieM84/VmbjXjCMfmI/AAAAAAAABLE/rVm4-80b0vg/s1600/Screenshot%2B2015-12-08%2B14.03.28.png)](http://1.bp.blogspot.com/-Lo25lMieM84/VmbjXjCMfmI/AAAAAAAABLE/rVm4-80b0vg/s1600/Screenshot%2B2015-12-08%2B14.03.28.png) |
| PIBUS IS WATCHING YOU                                                                                                                                                                                                                                 |

The display needs to handle various different situations, most obviously, when no data can be fetched from the API. Rather than get too bogged down in the details of whether our Internet connection is down, TfL's API servers are down, London is on fire, or it's just night time and there are no buses, I went for a simple message with a timestamp. Once this has been displayed, the code skips any further screen updates until it has valid data again. This makes it easy to see when a problem occurred.
<table>
<tbody>
<tr class="odd">
<td><a href="http://4.bp.blogspot.com/-p7P6uwowrWo/VmbeAe-0L4I/AAAAAAAABKY/OZm2eysWdAQ/s1600/IMG_7010.JPG"><img src="http://4.bp.blogspot.com/-p7P6uwowrWo/VmbeAe-0L4I/AAAAAAAABKY/OZm2eysWdAQ/s320/IMG_7010.JPG" width="320" height="240" /></a></td>
</tr>
<tr class="even">
<td>Maybe aliens stole the Internet, maybe it's a bus strike.<br />
It doesn't matter.</td>
</tr>
</tbody>
</table>

I also render a small timestamp on valid data screens too, showing when the last data fetch happened. This is mostly so I can be sure that the fetching code isn't stuck somehow. Once I trust the system a bit more, this can probably come out.

|                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<img src="http://2.bp.blogspot.com/-p0hx9zDklWw/VmbeBL08vWI/AAAAAAAABKo/YvtV2e9kKes/s320/IMG_7012.JPG" width="320" height="240" />](http://2.bp.blogspot.com/-p0hx9zDklWw/VmbeBL08vWI/AAAAAAAABKo/YvtV2e9kKes/s1600/IMG_7012.JPG) |
| The final design, showing a fallback for when there is data for  0 &lt; x &lt; 3 buses                                                                                                                                             |

|                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<img src="http://4.bp.blogspot.com/-xL33OieTago/VmbeBEhSrPI/AAAAAAAABKw/0tQRguImLeI/s320/IMG_7013.JPG" width="320" height="240" />](http://4.bp.blogspot.com/-xL33OieTago/VmbeBEhSrPI/AAAAAAAABKw/0tQRguImLeI/s1600/IMG_7013.JPG) |
| Data for three buses, plenty of time to get ready for the second one!                                                                                                                                                              |

So there it is, project completed! Grab the code from <https://github.com/cmsj/pibus>, install the requirements on a Pi, give money to the awesome Percheron Electronics for the E-Paper HAT (and matching PiBow case), throw a font in the directory and edit the scripts for the bus stop and bus route that you care about!
