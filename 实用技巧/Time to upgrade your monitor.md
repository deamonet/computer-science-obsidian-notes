-   [Blog](https://tonsky.me/)
-   [Talks](https://tonsky.me/talks/)
-   [Projects](https://tonsky.me/projects/)
-   [Logos](https://tonsky.me/design/)
-   [Patrons](https://tonsky.me/patrons/)

# Time to upgrade your monitor

![](media/cover.gif) Illustration by [Yulia Prokopova](https://www.behance.net/julia_litvin)

_Translations: [Chinese](https://sspai.com/post/61252) [Russian](https://habr.com/ru/post/508824/)_

I am a programmer. I do not deal with digital painting, photo processing, video editing. I don’t really care for wide gamut or even proper color reproduction. I spend most of my days in a text browser, text editor and text terminal, looking at barely moving letters.

So I optimize my setup to showing really, really good letters. A good monitor is essential for that. Not nice to have. A MUST. And in “good” I mean, as good as you can get. These are my thoughts, based on my own experience, on what monitors work best for programming.

# Low-density displays

According to [my research among programmers](https://twitter.com/nikitonsky/status/1260287480638701568), 43% are still using monitors with pixel per inch density less than 150:

![](media/research.png)

Why is this a problem? Because the only way to get good letters is by spending more pixels per letter. That simple. In the past, the displays’ pixel count was small, so we learned to live with that and even invented some very clever tricks to make our lives better. The two important things to understand are:

-   Times of low-resolution displays are over. High-resolution displays are a commodity now.
-   Tricks developed for low-resolution displays didn’t magically make text look good. That always was and still is impossible. They just made text slightly less terrible, but it still is terrible.

If you think that you can somehow make your 1080p display render good text, that it just needs a few more tweaks, NO. This won’t happen. The sooner you accept that the sooner you can start looking for real solutions.

To make my claim better founded, let’s look in detail how text _really_ looks on a low-resolution display and what can be done about it (spoiler: not much!).

## Not enough pixels

First, there are simply not enough pixels to draw a letter. Let’s take Consolas, a font that was developed specifically for programmers. Microsoft worked really hard to fine-tune it for low-resolution rendering. We set it at 14px, which is default in VS Code (and people often go lower than that!):

![](media/consolas_gray_1x.png) Consolas at 14px, macOS

On that size, capital letter B takes up mere 6×9 pixels on a screen. Lowercase letters only have 7 (seven!) vertical pixels to work with. That’s NOT MUCH. I have more fingers than that. No matter how good font is designed, it’s hard to show anything when all you have is seven pixels. Anything slightly more complex than “T” or “H” becomes an illegible pixel mess.

Look at the “g” in the picture above. It’s hard to say where strokes start or end, or even how many of them are there. It’s just random gray noise or a checkerboard, but not a letter. This is a letter:

![](media/consolas_24x.png) Consolas at 168px

It’s a shame, really, watching these beautiful fine details getting clamped into mere 7×10 pixels.

## The dreadful hinting

To fight the problem of everything being a gray mess, Windows uses pretty aggressive hinting. Basically, it just bends and moves letter strokes to the nearest pixel, ensuring more crisp boundaries.

And it works! Fonts do look better with hinting than without it:

![](media/hinting.png) No hinting (macOS) → Hinting (Windows)

Don’t get your hopes up though: it’s still a mess. It doesn’t make text look _good_. It makes it look _better_, but it is still bad.

The main problem with hinting, though, is that it destroys letter shapes. Pixels are rendered not where they are supposed to be, but rather where pixel grid happens. To give you a taste:

![](media/ttf_hinting.png) Verdana (k) and Times New Roman Italic (z) before rasterization at 13px. [Source](http://web.archive.org/web/20160304021305/http://antigrain.com/research/font_rasterization/index.html)

The idea is that it’ll look better when rendered to the actual pixels.

But even if we just look at vertical hinting of horizontal stems, it’s still changes font way too much:

![](media/hinting_align.png)

See how horizontal stems are offset from their actual position in the vector font file? The error here is as big as ¼ pixel!

But hey! If you never saw a high-res Consolas, who cares if `g` is the same shape or not? Who cares if stems are in the wrong place if you don’t know where they should’ve been in the first place? Well, sometimes problems are more obvious: circles are not circles, equal distances become not equal, proportions are all wrong, what supposed to be small becomes huge and vice versa, etc. Here:

![](media/hinting_numbersign.png)

After moving horizontal stems to match pixel grid (by offsetting them up to ½ pixel!), Windows is having a hard time splitting 7 other pixels into three equal gaps. Unfortunately, alternative is no better:

![](media/no_hinting_numbersign.png)

From my personal experience [building Fira Code](https://github.com/tonsky/FiraCode/issues?q=is%3Aissue+is%3Aopen+label%3Ahinting), I’ve seen too many ways simple idea “just stick edges to the nearest pixel” can go wrong:

![](media/hinting_failures.png)

![](media/hinting_failures_2.png)

This is a game that just can’t be won.

## A fractional pixel

Can you draw a perfect line that is thinner than one pixel?

Yes. The idea is simple, really. Your display’s pixel consists of three vertical subpixels, each responsible for their color. We can light them up individually, effectively tripling horizontal resolution!

![](media/subpixel_1.png)

In practice, though, you can’t implement it literally like that, because you’ll just end up with a christmas light mess:

![](media/subpixel_2.png)

So you have to compromise again (inside another compromise!), putting limit of how far can color deviate from black:

![](media/subpixel_3.png)

Which means letter shapes are not 3× sharper, they are maybe 1.5× sharper, but overall still pretty blurry.

![](media/subpixel_4.png)

In the end, there is an improvement in readability, but at the same time, black-on-white text gets a slight teal-and-orange halo around it. It’s not super bad, but you _can_ see it.

What I am trying to say is: all these tricks work. Having them is strictly better than not having them. For low-DPI displays all those are _a must_. But at the same time, they are a tight compromise made in times where we didn’t have better displays. Now that we have them, it’s time for all those tricks to go.

![](media/subpixel_5.png) Consolas 14px with ClearType and hinting → Consolas 14px @2x

# Retina Macbooks

Retina Macbooks _can_ make text look good. However, there are two things you absolutely must do.

## Turn off font smoothing

First, turn OFF “Font smoothing” in System Preferences → General:

![](media/no_smoothing.png)

I’m not sure what the default value for it is these days, but make sure it’s OFF anyways.

UPD: Based on the feedback, seems that default value for it is ON. Be sure to switch it OFF!

That preference name is misleading. It used to be called “LCD font smoothing”, which suggested subpixel antialiasing. But Apple removed subpixel antialiasing from macOS in 2018, the same month it retired its last non-retina laptop.

The other thing the name suggests is that your fonts might not be smoothed at all. This is not the case either.

What it actually does is it makes font slightly bolder:

![](media/smoothing_anim.png) Hover over or click the image to see the difference

So, why would you want it off? Because there’s no automated way to make font bolder. Normally each font weight is carefully designed by a professional type designer. It’s a tricky process that involves millions of constraints. If you try to simulate it by, for example, adding an outline to the letter, it will look terrible:

![](media/fake_bold.png) Real bold v. Fake bold simulated with an outline. Hover or click to see the outline

But that’s exactly what “Font smoothing” in macOS does! Here’s another example. MacOS blurs pixel-perfect boundaries with “font smoothing”:

![](media/smoothing_artefacts.png)

Imagine a font designer who carefully balanced every letter, placed each point down to 1/100 of the pixel, only to be ignored by dumb software that thinks it knows better.

What does that mean for us, programmers? If you take a font that was hand-optimized for particular pixel size (which many programming fonts are, e.g. [Input at 11px](https://input.fontbureau.com/info/) or [Monoid at 12px](https://larsenwork.com/monoid/)), it will be rendered blurry despite all the efforts.

![](media/smoothing_input.png)

And all the other fonts, including system ones, will be slightly more blurry than they need to be.

UPD: Chris Morgan [mentioned in a comment](https://news.ycombinator.com/item?id=23553486) that this setting might explain why so many designers put `font-weight: 300` [as their default web page font](https://grumpy.website/post/0PPp8l_pu). They are over-compensating for font emboldement of macOS!

UPD2: Big Sur seems to have removed the setting, but you might use a terminal command (reboot (maybe?) required):

```
defaults -currentHost write -g AppleFontSmoothing -int 0
```

UPD3: Alastair Byrne has released [a small tool](https://www.fontsmoothingadjuster.com/) to change the setting. Works on Big Sur!

## Integer scaling

When I bought my first (and world’s first) Retina Macbook Pro in 2012, it was exactly what was advertised: 2× scaling, every logical pixel rendered to 2×2 screen ones. 2880×1800 screen would be rendered from 1440×900 logical source.

Sadly, reason has left Apple since, and at some point, Macbooks started to get weird non-integer scaling as default. E.g. 2880×1800 screen would have 1680×1050 logical resolution. That is 1.7142857143… scaling factor, or 12/7.

Why? I guess, someone at Apple decided that more screen real estate sells better. The problem is, it’s not that big an increase: just a mere 15%. I mean, 15% is good, but not game-changing. The terrible part is, it comes at the cost of losing any chance to render ANY pixel-crisp image at all!

Let’s see. 12/7 scaling factor means that for every 7 logical pixels there are 12 corresponding screen pixels. Meaning, every 7 pixels you have a chance to draw a 7-pixel tall rectangle and that’s your only chance to align with the pixel grid.

![](media/12by7_grid.png)

Move 1 pixel up or down—you lose. Make it 1px taller or shorter—you lose.

![](media/12by7_grid_offset.png)

A pixel-perfect line? Too bad you can’t specify 7/12 of a pixel as the line width. Even worse, each 1px line looks differently depending on its vertical position:

![](media/12by7_lines.png)

It shouldn’t come as a surprise that modern icons are mostly made out of single-pixel-wide strokes:

![](media/12by7_ui.png) Top: 2× scale, bottom: same after 12/7 downscaling

It’s hard to imagine someone who wants to look at _that_ on purpose.

(no idea why bottom right pixel is missing on all the icons)

What happens with the text? Nothing good. First it is rendered pixel-crisp at 2× resolution, then downscaled at 85.7142857143…% to fit into physical pixels:

![](media/12by7_text.png) Monoid at 12px. Top: 2× scale, bottom: same after 12/7 downscaling

That’s right, UI isn’t even rendered in that weird target resolution. Every Mac app thinks it is rendering at 2×, and only after that OS scales it down to the target resolution. There’s a lot of precision and nuance lost due to this two-step resize process.

In my opinion, nothing can do more harm to the look of UI than this. Even old low-dpi UIs are better since their lines at least align with pixels!

And don’t forget: this is the default. Every Macbook is shipped with these settings. Millions of people are working not knowing they were robbed of the joy of the retina screen.

Luckily for us, this is easy to fix (at least for now). Go to System Preferences → Displays, uncheck Default and select 2× resolution instead:

![](media/scaling.png)

This will make everything on the screen slightly bigger, leaving you (slightly!) less screen estate. This is expected. My opinion is, a laptop is a constrained environment by definition. Extra 15% won’t magically turn it into a huge comfortable desktop. But at least you can enjoy that gorgeous screen and pixel-crisp fonts. Otherwise, why would you buy a retina screen at all?

## ClearType on Windows

With all this talk about downsides of ClearType and how it’s only a must on low-density displays, should you turn it off on 4k display? In theory, yes. In practice, no.

First of all, Windows doesn’t even have a UI to turn it off. I mean, there’s this checkbox:

![](media/windows_cleartype.png)

but even if you turn it off, you’ll have to go through configuring ClearType anyway. There’s just no OK button ¯\_(ツ)_/¯.

If you turn it off this way, it’ll disappear in some places, but keep appearing in others. I guess those places use different APIs and one respects this setting while other does not.

![](media/windows_cleartype_mix.png)

And most importantly, text without ClearType looks like shit. It doesn’t have to (it looks perfect on macOS, for example), but particularly on Windows it’s unbearable. I guess they don’t even test this as an option:

![](media/windows_no_cleartype.png)

Just for fun, I retyped all the text labels using the same font, size and color, but on macOS:

![](media/mac_no_cleartype.png)

But ClearType-d text on Windows still looks good, even on 4k display. It’s just a shame we can’t let ClearType go yet.

# Get a monitor

Let me express an opinion. This is my blog, after all. I think that laptops are not good for development. They are great in mobility and convenience, and this argument might outweigh everything else for some people. I accept that. But still, a desktop monitor + external keyboard are _always_ better than a laptop. There might be other reasons for not buying a monitor, but having one, I hope, no one would argue it’s a superior dev environment.

With this out of the way, the question arises, which monitor should you get? From what we already discussed, two things should be clear:

-   It needs to be at least a 4k monitor. Both 5k and 6k are also great, of course.[1](https://tonsky.me/blog/monitors/#fn:1)
-   You need to use the integer scaling factor.

That means, if you have a 4k monitor (3840×2160), and use 2× scaling, you’ll get an equivalent of 1920×1080 logical pixels. So it’s a basic 1080p monitor in terms of how much you can fit, but with much crisper UI and text in everything.

Now, it might be tempting to use, for example, 1.5× scaling. That would give you an equivalent of 2560×1440 logical pixels, which, you might think, is much better. This is not how you should use it! The idea of a 4k monitor is NOT to get more pixels but to get the pixel-perfect, high-density UI rendering. Otherwise, a normal 1440p display would work better. A simple rule to remember: _pixel alignment outweighs everything else_. 1440p display is better at displaying 1440p content than 2160p display is at it.

It’s also possible to run a 4k display at native 3840×2160 pixels. It depends on the size of the display, of course, but in my experience, even 27” 4k displays are too small to run at 1×. The UI will be too tiny.

## The myth of Apple-patented PPI

[Some articles](https://bjango.com/articles/macexternaldisplays/) suggest that Apple computers should only be used with 220 PPI displays (pixels per inch) because that’s the number Apple itself uses on all the Macbooks and iMacs. Sometimes people go as far as saying displays with different PPI are unusable with macOS.

This is what I think. PPI defines the physical size of the pixel (220 PPI means there are 220 pixels per inch, or 1 pixel is 1/220 inch wide). So Apple ensures that pixels on all its devices have the same size. Does it mean macOS controls have the same physical size? Not anymore, after Apple started applying non-integer scaling by default on Macbooks.

Then, it’s nearly impossible to ensure that _perceived_ size, or how big the user _sees_ the control, is the same because the distance to the display is different. For example, on average my eye-to-screen distance is 33 cm with the laptop, but 68 cm with the monitor. That’s 2× difference!

![](media/distance.png)

That means that the angular size of the 1/220 Macbook pixel is equivalent to 1/110 monitor pixel. I’m actually having smaller _perceived_ pixels on 4k 27” monitor than I have on 15” Macbook Pro!

Even Apple itself understands that! Their iPhones have higher PPI than Macbooks because they are usually looked at from a closer distance.

To sum things up, I don’t see a problem with 24” 4k displays or even 27” displays. I use both with macOS and love both, never had any problems. Of course, 5k or 6k would be better, but those go in the “nice to have” category. 4k is a must-have, an absolute minimum for anyone working with text.

# Go 120 Hz

The world used to be divided into two camps: high-resolution displays and high-frame-rate displays. The former was good for text, latter for gaming, with no middle ground in between. If you like to play competitive games, buy both (and a large table). Gamers had no use for 4k displays since no reasonable game would run at 4k @ 120Hz, and the creative professionals had no use for 120 Hz in photo/text editing. I was in high-res camp since 2014, of course, and would never trade retina text rendering for barely noticeable refresh rate upgrade.

![](media/camps.png) HP Z27 (4k) v. LG 34GL750-B (120 Hz)

Well, the split does not exist anymore. Since not that long ago (yes, I’m too lazy to check) you can have both! You can have a 4k monitor that runs on 120 Hz. In fact, that discovery was the main motivation for this article.

## Why 120 Hz?

If you, like me, work with text, you might think that you have no use for 120 Hz. And you would be right. This falls into a “nice to have” category, but if you are looking for ways to improve your experience, this is a great way to do it.

120 Hz gives you a couple of significant improvements:

-   Animations are smoother, up to the point where they start to appear like a continuous motion instead of a very fast slideshow.
-   Scrolling is very smooth in particular. Browser, code editing, to name a few.
-   The whole system feels much more responsive.
-   You can play games and work on a single display.

I can’t show you how 120 Hz feels, of course. But this is what you can do to get the idea: switch to 30 Hz and try to work like that for a while.

You’ll notice that everything is poorly animated, but also way less responsive. This is because the time between monitor updates is now 32 ms instead of 16 ms at 60 Hz. That means that whatever you do (press a button, move a mouse), the closest moment in time computer might start displaying the result might be as far as 32 ms away.

32 ms is a long time and easily perceivable. On 60 Hz, that time is cut in half: the longest you need to wait is just 16 ms. On 120 Hz, this is time is cut in half again: from 16 ms to 8 ms. In absolute numbers, you lose an additional 8 ms, which means going 60 Hz → 120 Hz is about half as impactful as going 30 Hz → 60 Hz. Still, something to go after, in my opinion.

## What to buy?

There isn’t much choice, really. From what I can find, right now there are only four (yes, four!) 4k 120+ Hz displays on the market! I imagine that’s because demand is not that high, but hey, I’m glad we have at least those!

First one is [Asus ROG SWIFT PG27UQ](https://www.asus.com/us/Monitors/ROG-SWIFT-PG27UQ/):

![](media/asus_pg27uq.png)

Second one is [Acer Predator X27](https://www.acer.com/ac/en/US/content/predator-series/predatorx27):

![](media/acer_x27.png)

Third one is [Acer ConceptD CP7](https://www.acer.com/ac/en/US/content/conceptd-model/UM.HC1AA.P02):

![](media/acer_cp7.png)

All are very nice monitors, I’m sure. But the price is a bit too steep (~$2,000), especially for someone for whom getting 120 Hz is not a question of life and death.

There are few more [here](https://www.tomshardware.com/reviews/best-4k-gaming-monitors-pc-144hz,6023.html) with a diagonal size of 55” and more, which would be hard to use on a normal desktop.

Finally, by some unbelievable struck of luck, we actually have ONE reasonably priced, reasonably sized 4k 120 Hz monitor. That’s [Acer Nitro XV273K](https://www.acer.com/ac/en/US/content/model/UM.HX3AA.P02):

![](media/acer_xv3.png)

And that’s the one that I have.

## Things to look out for (Windows)

Running 4k at 120 Hz is simple on Windows. Make sure your graphics card has DisplayPort 1.4, use it, that’s it. Seriously, it just works.

![](media/windows.png)

## Things to look out for (macOS)

MacOS support sucks. Officially none of the Apple computers support anything beyond 60 Hz, even on normal resolutions:

![](media/macs_compatibility.png)

So buying this display was the pure leap of faith. This is what I figured:

-   4k @ 120 Hz requires 3840 × 2160 × 3 bpp × 120 Hz × 8 = 24 Gbit/s. Just below of [25.92 Gbit/s of DisplayPort 1.3 / 1.4](https://en.wikipedia.org/wiki/DisplayPort#1.3).
-   HDMI 2.0 only provides 18.0 Gbit/s, so it has to be DisplayPort.
-   Thunderbolt 3 can carry DisplayPort 1.4, so if only I can find the adapter, I should be good to go.

Which port does my Macbook have? Easy! Use this Apple-provided diagram:

![](media/ports_diagram.png)

Okay, so lighting bolt icon means Thunderbolt (not to be confused with Lighting port!), railroad switch means USB-C. Now just look at your Macbook:

![](media/macbook_ports.png)

I guess you just have to know it ¯\_(ツ)_/¯. Alternatively, one can look at the Apple’s [intuitively named page SP794](https://support.apple.com/kb/SP794?locale=en_US):

![](media/macbook_spec_1.png)

![](media/macbook_spec_2.png)

Ok, first of all, what does Thunderbolt 3 (USB-C) mean? Is it Thunderbolt 3 or is it USB-C? This might be a difference between “works flawlessly” and “doesn’t work at all”:

![](media/monitor_ports.png)

Then it says “DisplayPort over USB-C” (but we have Thunderbolt 3, not USB-C!). It does not specify the DisplayPort version, and it’s useless without it. It also says USB 3.1 Gen 2 is limited by 10 Gb/s, but I guess USB 3 limits do not apply to USB-C? Also, what kind of name is USB 3.1 Gen 2? Was USB 3.2 already taken?

Well, [Wikipedia to the rescue](https://en.wikipedia.org/wiki/Thunderbolt_(interface)#Thunderbolt_3)!

![](media/thunderbolt_3.png)

Turns out Thunderbolt 3 [might or might not carry DisplayPort 1.4](https://en.wikipedia.org/wiki/Thunderbolt_(interface)#Thunderbolt_3). Some of them only go as far as DP 1.2. The Wikipedia article suggests that anything before 2018 definitely won’t work, but after 2018 it might or might not work, depending on a Thunderbolt version. Feeling lost? Use my diagram:

![](media/thunderbolt.png)

I think we all can agree that this whole Thunderbolt/USB-C situation is a very strong contender for the “Most confusing port standard ever created by humanity”.

Long story short, I was lucky. My Macbook Pro 2019 had the correct port and with Thunderbolt 3 (USB-C) to DisplayPort adapter, everything worked. My understanding is, port versions on devices matter, but cables and adapters do not, as long as they physically fit into the hole. In my case, it was [Xiaomi USB-C → miniDP](https://www.mi.com/commutator-dp) converter and miniDP → DP cable.

![](media/converter.png)

Will it work for you? No idea! I hope so. All I know is that you should make sure your Thunderbolt 3 can carry DisplayPort 1.4. That’s the magical combination.

## Things to look out for (macOS)—continued

If that was not confusing enough, there’s more!

Your Macbook has to have a discrete graphics card (I think).[2](https://tonsky.me/blog/monitors/#fn:2) Various Intel UHD / Iris Graphics do not work. eGPU works.

![](media/graphics.png)

But even if you have the compatible Mac, with compatible ports, compatible cables, this is not enough. Every time I boot my Mac, there’s a ritual I have to perform to get my display to switch to the 120 Hz mode. I call it “120 Hz dance”:

1.  Fully boot into macOS. At this point display usually is at 60 Hz.
2.  Go to System Preferences → Displays.
3.  While holding down Alt / Option key (the one with ⌥ on it), click on “Scaled” resolution.
    
    ![](media/settings_1.png)
    
4.  To get access to Refresh rate chooser, check the “Show low resolution mode” checkbox. L for logic.
    
    ![](media/settings_2.png)
    
5.  Look inside “Refresh Rate”. Most of the time the highest option there is “60 Hz”.
    
    ![](media/settings_3.png)
    
6.  Turn display off.
7.  Wait a couple of seconds.
8.  Turn the display on.
9.  Look again under “Refresh Rate”. Hopefully, there’s now a “119.88 Herz” option.
    
    ![](media/settings_4.png)
    
10.  Select “119.88 Herz” in “Refresh Rate”.
11.  You are magnificent.

Why is it 119.88 Herz, not 120 Herz? No idea. It seems to work the same. Why can’t macOS remember it? I don’t know. Why doesn’t macOS sees 120 Hz as an option until I turn the monitor off/on? Who knows! The main takeaway is, 120 Hz option might not always appear, but after some dancing around it might, and if it does, it actually works, despite all odds.

The whole situation reminds me of buying 4k display in 2014: there are only a couple of models, ports are confusing, Apple support sucks. Hopefully, in five years 120 Hz will become a standard. Until then we should be grateful that, at great inconvenience, we at least can use modern displays with macOS. Thanks, Apple!

UPD 2020-09-24: 2019 MacBook Pro 16 with Radeon 5500M supports external monitors at 4k @ 120Hz and even 30-bit color over DP no sweat! This seems like a lowest MacBook model you should look out for.

UPD 20201-02-24: I started building up [a list of Macs/Monitors compatibility](https://tonsky.me/blog/monitors-mac/). Check it out and help me update it!

# What’s next?

Every human needs a dream. At some point, 4k @ 120 Hz will become the commodity, and we might even see 5k @ 120 Hz and more. We might also see Retina screens with ratios of 21:9[3](https://tonsky.me/blog/monitors/#fn:3) and even 32:9 (more horizontal space), which is always a welcome addition.

But even today you can peek into the future, if you have extra $4,000 to spare. This is [Dell UP3218K](https://www.dell.com/en-us/work/shop/dell-ultrasharp-32-8k-monitor-up3218k/apd/210-alez/monitors-monitor-accessories), world’s first and only 8k monitor:

![](media/dell_up3218k.png) Even on a promo page for 8k display, Dell puts out only 1× photos of it.

The pixel density on it is so high (280 PPI) it’s probably best used at 300% scaling (which macOS doesn’t have, of course, but Windows does). It also requires TWO simultaneous DisplayPort cables to work, which is again, a no-go for Macs.

But even at 300% it will still give you an effective logical resolution of 2560×1440, which is substantially more than 1920×1080 of modern 4k displays. More pixel density and bigger resolution! Well, one can dream.

# Conclusion

To sum up, this is the best setup for programmers:

-   Text can’t be made look good on low-resolution displays.
-   High-PPI displays are now a commodity, it’s time to switch.
-   Laptops are ok, but a standalone monitor is always better.
-   4k monitor only makes sense with 2× / 200% scaling.
-   If you want to go further, there are now affordable 4k @ 120 Hz options.

Happy coding!

1.  Except for LG 5k, this one [is haunted](https://steipete.com/posts/the-lg-ultrafine5k-kerneltask-and-me/). [↩](https://tonsky.me/blog/monitors/#fnref:1)
    
2.  The might be other reasons affecting this, like Thunderbolt revision. I have a limited test base, but: Macbook Pro 15” 2019 works, Macbook Air 2018 does not, Mac mini 2018 only works with eGPU. [↩](https://tonsky.me/blog/monitors/#fnref:2)
    
3.  Actually, there’s an impressive [34WK95U-W](https://www.lg.com/us/monitors/lg-34WK95U-W-ultrawide-monitor), but you can also see it as a shorter version of more traditional [27MD5KL-B](https://www.lg.com/us/monitors/lg-27md5kl-b-5k-uhd-led-monitor). [↩](https://tonsky.me/blog/monitors/#fnref:3)
    

June 17, 2020 · Discuss on [HackerNews](https://news.ycombinator.com/item?id=23551983)

Hi!

I’m Nikita. Here I write about programming and UI design[](https://tonsky.me/blog/how-to-subscribe/)

[Subscribe](https://tonsky.me/blog/how-to-subscribe/)

I also create open-source stuff: Fira Code, AnyBar, DataScript and Rum. If you like what I do and want to get early access to my articles (along with other benefits), you should [support me on Patreon](https://patreon.com/tonsky).