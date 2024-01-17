
# Film X-rays for reverse engineering

This was a joint experiment conducted by Aleksandar Nikolic and Travis
Goodspeed, presented at ShmooCon in DC on 13th of January 2024.  This
writeup serves as a log documenting all the relevant details.

<img src="https://github.com/ea/film_xrays/assets/65802/933ffc4e-2308-44fc-bc1b-e98b727b293b" width="600">

## The Pitch


A cabinet x-ray machine is a handy tool for any reverse engineer of
electronics, but the price tag keeps convenient x-ray photography
beyond the reach of most hobbyists, particularly when the digital
sensor works.  In this lecture, we'll show two film alternatives to
digital photography, generating x-ray pictures first with a proper
dark room under a red light and second with polaroid/fujiroid film in
the absence of wet chemistry.  We'll also explain where film can be a
better alternative to a digital sensor, offering better resolution and
dynamic range at a much larger surface area.

## The machines
<img src="https://github.com/ea/film_xrays/assets/65802/f8fe50e0-e1b5-4ad4-9406-88dd2f73a0ce" width="600">


"Cabinet" means that the imaging area is completely enclosed,
shielded. They usually have relatively low x-ray energies (35 kVp)
which doesn't require a lot of shielding making them very safe to use.
Popular among independent electronics engineers are a few vintage
models from Faxitron: mx-20 and dx-50. Most important feature of these
machines is their micro-focus x-ray source. This directly translates
to high resolving power, essential for studying small features.

When these were originally produced, they were offered with a few
different imaging options. The imaging part of these machines is
completely independent in operation from the machine itself. Most
common options were either 50x50mm or 120x120mm hamamatsu CMOS based
detectors. If you think about it, these are basically large format
camera sensors which makes them extremely expensive, even to this
day. The sensor itself easily constitutes the bulk of the machine
cost.  It is not uncommon to run into a machine where a sensor is
missing or broken but that still produces x-rays.

<img src="https://github.com/ea/film_xrays/assets/65802/5db792f2-eb2a-4452-975f-5bed8fadee2b" width="600">


## Motivation

Missing sensor makes them almost useless, but a crafty individual can
still use them to produce x-ray images the old way, with film and
chemicals. This sounds like it'd be messy, complicated and like it
would require expensive equipment - goal of thIs guide is to show you
that it's actually easy to do.  We'll go through two recipes. One that
produces high quality images but is a bit involved. And another, that
sacrifices quality for speed and ease and makes for a fun party trick.

The machine in my possession is a Faxitron DX-50, the smaller
model. It's imaging area is roughly limited by the diameter of the
chamber which is about 200mm. This is a bigger area than 120x120mm
sensor and much bigger than the more common 50x50mm sensor.
Coincidentally, standard size 4x5 medium format photographic film fits
comfortably with room to spare.  This means that, by using large
format film, we can get a much larger imaging area.

As already mentioned, digital sensor works independently of the x-ray
machine. In simple terms, software that controls them both tells the
machine to start shooting x-rays and then separately starts capture on
the sensor. Sensor has different gain settings but relatively short
maximum exposure times. Software usually makes a couple of exposures
with different settings that then get merged. This translates into
relatively low dynamic range. In other words, if you are imaging
something that has very thin and very thick parts, you might have
trouble seeing both in the end result.  Modern black and white film
has incredibly wide dynamic range which, coupled with long exposure
times, can lead to more detailed results.

Additionally, even the bigger sensor (hamamatsu C9732DK) has limited
resolution (2400x2400px) for the size of the sensor. A modern,
low-speed, fine-grain film can record much finer details.

Of course, using film has drawbacks. It can be a bit messy and the turnaround time is not exactly short… 

## Film supplies

For my experiments, I've decided to use Ilford Ortho Plus film in 4x5 format. 

<img src="https://github.com/ea/film_xrays/assets/65802/fb3e184d-170a-429d-aba1-525132e0f391" width="600">



It's very popular, still produced and easy to find. It's a technical,
fine grain, film with ISO rating of 80 which makes it "high
resolution". Ortho in the name stands for orthochromatic which means
that it's not sensitive to deep red light , meaning that it can be
handled safely in a darkroom under safe light. This last bit isn't
crucial, as the recipe we will outline doesn't require a dark room,
but it's certainly convenient.

Besides the film, you will need a couple of things to develop it. My
goto black and white developer is Kodak HC-110 which is extremely
inexpensive, versatile and available everywhere. Fixer is fixer and
doesn't really matter but my choice is Ilford RapidFix.  ￼ ￼ ##
Additional equipment

If you have a darkroom, or a particularly dark bathroom or closet, you
can get away with most of these, but they are handy and allow you to
do film processing anywhere.
- Changing bag
- Development tank or development trays
- Film holder
- Bottles and measuring cups for mixing chemicals. 

<img src="https://github.com/ea/film_xrays/assets/65802/65c744e3-62ab-497f-a73b-57cb60b23521" width="600">



## Exposure time 

The imperative in radiography is to minimize the patient x-ray dose as
much as possible. This is done through a combination of short exposure
times, fast films and intensifier screens. Specialty X-Ray film is
designed to be very sensitive at the expense of resolving power. It is
usually coated with emulsion on both sides to further increase the
speed. Additionally, most x-ray film is made to be very sensitive to
green light as it is made to be used in cartridges that have built in
intensifying screens. An intensifying screen is simply a piece of
coated paper that fluoresces green when hit with x-rays. This further
reduces necessary exposure times, but further decreases image quality.

We are concerned with imaging inanimate objects, so we can forgo
trying to limit the patient dose. This simplifies our job as we'll be
exposing film to x-rays directly, in very close contact with the
object being imaged. We just need to keep it safe from all visible
light. To do so, we need to fashion a light tight, but x-ray
permeable, film holder. I've made a simple 3d printable design that
uses a piece of thin, black opaque plastic. You want to make sure that
whatever material you are using isn't absorbing too much x-rays. You
can get away with few layers of PETG (surprisingly, PLA seems to be
more absorbant), but the thinner the better. I used the same material
that film bags are made of. The only important part is that the whole
film holder is completely light tight.

Now that we can safely keep the film out of light, we need to figure
out the exposure time. There isn't really a practical way to estimate
this, but I found 2 minutes to be good for most of the
subjects. Thicker, multi-layer boards might need more time.

Yes, this is A LOT of exposure compared to relatively short exposures
required by the digital sensor, but so it goes…  For my experiment's,
I've been using `xray.py` script from John McMaster's repository. It
allows you to set the time, set the kVp and fire the machine. Since
35kVp is already at the edge of what's practically usable for imaging
PCBs , I never saw any benefit to using anything lower than that.

## Procedure 1: Black and White film

### Prep step 1 - Chemical solutions

<img src="https://github.com/ea/film_xrays/assets/65802/f3f2ac48-1f34-449f-b9cb-81d7351c302f" width="600">

You will need to prepare enough chemicals for the size of the
development tank that you have, but the ratios stay the same.  For
developer, HC-110, the default is the so called "Dilution B" which is
1:31 dilution of concentrate with water. That is, 1 part concentrate
to 31 parts of water.

For fixer, standard dilution is either 1+4 or 1+9 for 1 part fixer to
either 4 or 9 parts of water. There's no downside to using higher
dilution, except slightly longer wait times, so I tend to save a bit
and use a more diluted solution.

For the steps in-between developing and fixing, so called stop bath, plain water is fine. 
Clearly mark the bottles, if you mix up the order your development will fail. 

### Prep step 2 - Load the changing bag
<img src="https://github.com/ea/film_xrays/assets/65802/0deaaf2f-17a1-4047-8e70-84a792408914" width="600">


Second step is to transfer the film from its original packaging to the
slide holder that goes into the x-ray machine without exposing it to
light. To do so, you load the following into the changing bag:
- Pack of film
- Film holder 
- Film holder clips

Zip up the film bag, push your hands through the sleeves and load the
film. Take one sheet out of the film pack, center it into the holder
(emulsion side up preferably, but the x-rays don't care that
much). Use the clips to make the holder light tight.  Before opening
the bag, make sure that the film pack is closed and secure, lest you
expose all the film.

<img src="https://github.com/ea/film_xrays/assets/65802/c3a71a08-0537-4eea-ab7d-b6b3e0514121" width="600">


### Prep. Step 3 - Prepare for exposure

<img src="https://github.com/ea/film_xrays/assets/65802/9882f8a5-de92-4858-b590-463704318c45" width="600">


Carry the prepared film holder into the X-ray machine and position it
in the center of the chamber. Put the object-to-be x-rayed on top of
the slide holder, trying to make it lie parallel.

### Exposure 

Turn on the machine and wait for warmup. Using `xray.py` set exposure
time to 120 seconds and kVp to 35. When everything is ready, issue the
fire command and wait two minutes until the microwave oven tells you
it's done.

### Prep step 4 - Readying the bag for film transfer
<img src="https://github.com/ea/film_xrays/assets/65802/3c211062-6000-4faf-9cf8-778a4cacc1c3" width="600">


The film is now exposed and we need to safely develop it. Load the
following into the dark bag:
- Slide holder 
- Development tank

Zip up the bag, pull your hands through the sleeves and carefully
transfer the film sheet from the slide holder to the film carrier and
into the development thank. Shut the tank close and make it light
tight. The rest can be performed in daylight.

### Developing the film

Film is now safely loaded into development tank and is ready for
processing. The rest of the steps can be performed under regular
lighting with no fear. We need to perform the following steps:
- Develop 
- Stop 
- Fix 
- Rinse

Development time depends on your choice of film and developer. It also
depends on the exposure, and since we are dealing with x-rays
manufacturer recommended development times aren't really
helpful. After some trial and error, I've concluded that with hc-110
in dilution b 6 minutes seems to be a good enough developing time.

The thank is light tight. You open the cap and pour in the developer
solution. You then proceed to agitate (turn upside-down and back) the
tank for first 30 seconds and then for 10 seconds every minute.

After 6 minutes have passed, pour out the developer and pour in
water. This acts as a stop bath. Older films had thicker emulsions and
required stronger, slightly acidic stop baths, but plain water is
usually fine for this purpose. Agitate the tank a couple of times and
pour out the stop bath.

Next up is fixing. Pour the fixer into the tank and follow the same
procedure as with development, agitating the tank for 10 seconds every
minute but for about 5 minutes. Don't stress this part too much as
film can be put back into fixing bath again if it was
under-fixed. After the initial fixing, it is effectively no longer
light sensitive (unless subverted to additional time in developer
solution), so it is safe to examine it under regular light.

After fixing, the film should look mostly clear with no milky parts,
but should clearly show a negative image. Last step is to rinse it
with water again. If you want slightly better drying, adding a drop of
mild soap or other wetting agent into the rinse water can help but
isn't strictly necessary.

After thorough rinsing, hang the film to dry in (as much as possible) dust free area. 

<img src="https://github.com/ea/film_xrays/assets/65802/4abc86b9-e4d0-4ad1-a362-c9fc0503a70d" width="600">


## Samples and results

After drying , the developed film negatives can be scanned of
photographed with a regular camera against a white light and then post
processed with regular photo software.  Here's a few examples:

<img src="https://github.com/ea/film_xrays/assets/65802/6862f1ce-6739-451d-b347-c3d99f3144d7" width="600">

An SmartResponse XE device. Above image shows a wide dynamic range
from completely opaque screws to very light, thin, plactic.

<img src="https://github.com/ea/film_xrays/assets/65802/780a32c5-0a49-4a68-8b7a-47a600b1d566"
width="600">

Defcon badge from 2022. 

<img src="https://github.com/ea/film_xrays/assets/65802/f621beb0-fc32-48c6-91a2-ce5ed470581e" width="600">

An example of imaging a much thicker board. Big one is a thunderbolt
dock. The board comtains many layers and has a heavy ground
plane. Imaging this on film is a bit of a struggle, but can be
acomplished with mdoified chemistry.

<img src="https://github.com/ea/film_xrays/assets/65802/6b472fb3-4806-4284-8805-c8e034ee9ad2" width="600">

Here's a closeup of an SD card that really shows the ammount of
details that can be achieved with this method. Even though silicon is
mostly invisible, you can see where the bonding wires terminate.

## Varying exposure and dev procedure

Through experimentation, we've determined few factors that help get
better results from challenging subjects such as boards with thicker
coper pours or too many layers.  First, extending the exposure time to
3 or more minutes can help, but also has limits. X-rays scatter and
"fog" the film.

Another option to get higher contrast from dense targets is to modify
the development chemistry. Mainly in the way of using a more
aggressive developer.  Photo paper developers are much stronger and
faster acting, which can be to our advantage when trying to lift very
light shadow details.  A good option in our experimentation was
Illford PQ Universal developer in standard dilution. It's fast acting
and the time film spends in the developer is very short. From 1:30 to
2 minutes. The results are negatives with higher contrast, and higher
grain, but with more details shown in thick boards.  My recomendation
would be to first try the standard proposed recipe and then experiment
with PQ developer if neccessarry.

Fixing step is always the same. 


## Procedure 2: Simpler method with Polaroids (fujiroids/instax film)

I hope I've shown that film developing isn't all that hard and messy
as it might have looked like.  Nevertheless, there's another option
we've experimented with that's much simpler. Albeit at the expense of
quality.  Polaroids have been around forever and are loads of fun,
even if they can be a bit pricey per photo. My only gripe with
polaroids today is that they are mostly very small.

As an alternative to Polaroid, Fuji also manufactures instant photo
film (fujiroid) and they have several formats. Biggest of them is
"fuji instax wide" which is just slightly smaller than previously
shown 4x5 film.

## Sidenote: How are instant photos developed?

<img src="https://github.com/ea/film_xrays/assets/65802/04cf21d5-cf3f-4a4b-bad6-1ba3ad63b92c" width="600">



Each instant photo/slide has these 3 pouches at the bottom that
contain development chemicals.  When a photo is taken, film is ejected
from the camera and is passed between rollers that squish the pouches
and spread the chemicals over the film. The reactions are timed so
that fixing kicks in after development is finished and all is done in
a matter of minutes. It's really fascinating achievement!

This basically means that you can develop polaroids on a sturdy
countertop with a rolling pin if neccessarry.

## Printing fuji instax
<img src="https://github.com/ea/film_xrays/assets/65802/a512782b-a6a0-4e8d-b287-e92500479952" width="600">


In order to avoid wet development (and rolling pins), we can either
reuse a regular instax camera or acquire one of these instax printers.
These are nifty devices that you can use to print digital images onto
instax film from your phone. But, nothing is stopping us from using it
to process the film that hasn't been exposed to regular light.

The procedure for our use case will be to take the instax film that's
been exposed to x-rays, put it back into instax pack and load it into
the printer.  When a "new" film pack is loaded, printer first ejects
the "dark slide". Since we've just put the exposed film into it, this
will effectively push the film through the rollers which develops it.

### Prep step 1: Preparing the instax film for exposure
<img src="https://github.com/ea/film_xrays/assets/65802/b53d4de9-3397-48cd-bb55-c2e748d57f81" width="600">


Into the dark bag go:
- X-ray film holder
- Holder clips
- Instax film pack 
- Light tight box for instax film pack
- Small flashlight

In the dark changing bag, open the film pack, take out the dark
slide. Take out one piece of film.  Quickly turn the flashlight on and
off, exposing the film. This is counterintuitive, but crucial for
instax film. This pushes the film over its activation threshold which
makes it more sensitive to all light and makes it possible to expose
it properly with x-rays. A very brief moment with not too bright
flashlight is enough.

After this short exposure, load the film into film holder and clip it
light tight with clips. The slide is now ready.  Still in the bag,
safely put the rest of the instal film pack into a dark light tight
box to protect it from exposure.

<img src="https://github.com/ea/film_xrays/assets/65802/17b7a6fa-adad-42bf-b86e-37fd2e96b838" width="600">



### Prep. Step 2 - Prepare for exposure
<img src="https://github.com/ea/film_xrays/assets/65802/a7063d2f-a991-413b-9a0a-70d1e2a7bb67" width="600">


As before, carry the prepared film holder into the X-ray machine and
position it in the center of the chamber. Put the object-to-be x-rayed
on top of the slide holder, trying to make it lie parallel. Remember
that the instax film is smaller than 4x5, so imaging area is smaller,
too.


### Exposure 

Like before , turn on the machine and wait for warmup. Using `xray.py`
set exposure time to 180 seconds this time and set kVp to 35. When
everything is ready, issue the fire command and wait 3 minutes until
done.

### Developing the film
<img src="https://github.com/ea/film_xrays/assets/65802/6b88e319-14d0-4cb3-99c5-cac889dec1c2" width="600">


With the film exposed, into the bag go:
- Film holder with exposed film
- Instax printer or instax camera
- Empty instax pack 

With everything in the bag, and the bag light tight, take the film out
out the film holder and put it into an empty instax pack, making sure
to orient it properly. Next, load the instax pack into the
printer/camera. Once you close the printer, it will automatically
eject the first, and in this case, only slide. Pushing it out starts
the film development which is completed after a couple of minutes.  Do
not, no matter what silly pop songs tell you, shake it like a polaroid
picture. It doesn't actually help.

## Samples and results

Results achieved with this method are much quicker but maybe less
practically useful for complex PCB reverse engineering.  They do,
however, make for a very fun party trick.

<img src="https://github.com/ea/film_xrays/assets/65802/c1e83b46-cfc9-4945-9e2d-37d691c5a5e8" width="600">

This one shows different smaller devices. A yubikey, an SD card, Pi
Pico and a casio digital watch board.  While these are clearly of a
lesser quality than the ones made on regular photographic film, they
can still be used to identify locations of various components for
example.

## Conclusion

We hope to have shown that not only is developing film not hard , it really does have merit of its own in certain situations. Especially when imaging larger objects that don't fit the digital sensor. 

