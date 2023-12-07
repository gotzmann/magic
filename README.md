# Magic X

I always dreamed of some project dealing with original hardware and software development right from the bare bones level. And it looks like your very own Game Console might be the one of coolest possible thing to do in that realm, right?

So a year ago I've started looking around and tinkering with some nostalgia consoles like SNES Classic Mini (just a mini box with some Nintendo titles inside) and Analogue Sega (much better thing built on FPGA and working with real caartridges) as well completely new beasts like completely virtual (call it FANTASY) console PICO-8. Yep, I've even bought my copy for REAL $15.

# CPU

There were two popular CPUs gathered iconic status over those hippie years: MOS 6502 and Zilog Z80. First was designed after a Motorola 6800 and second is after Intel 8080 which were not so popular within developers. Ugh, how's that was even possible?  

The MOS 6502 was used within 8-bit Atari arcades, mini-computers (400/800, XL/XE), Apple II, Commodore 64/128, Nintendo NES (and many-many others, of course).

The second went straight into Sinclairs, including ZX Spectrum, Amstrad family and some others, popular mostly within Japan and Europe.

I used to own and play with many 6502-based systems including Soviet clone of Apple II, Asian replic of NES and real Ataris and Commodores that were popular at computer clubs of USSR.

So after some research on history I've decided to use 6502 as my target CPU for brains of Magic console.

# 10-bit

Yeah, I've decided to build the thing with 8-bit processor in mind, but, man... there so many unnecessary complication, burden and mess even doing simple things with those older chips, just stopping the progress and creativity! Like when dealing with the screen where you'd like to calculate {X,Y} coordinates of your hero and messing with two additions instead just one, cause the screen width is 320 but you have only max 256 available within one byte.

So after some thought I've decided to try something esoteric. How about 10-bit CPU in your machine? I'm sure that's the FIRST 10-bit CPU in the world! Remember, I'm going to have a real thing with some real 10-bit CPU based on FPGA maybe?

What the pros? Numerous! 

Now we are able to calculate anything within 1024 with just one operation and one register. Any screen corner becomes easily accessible.

Next, combining two 10-bit registers, we jumping far above 65K memory limit. No paging needed anymore. 20-bits allows us to address 1Mb memory. How cool is that? Much better graphics and audio will be easily avaiable for game developers.

How many more colors we might use with 10-bit instead of ugly 16 old school color-pallettes? Stealth bit + 9 RGB bits = 1 + 3R + 3G + 3B = 512 colors!

# Screen

Most of 8-bit consoles and computers had screen resolutions around 300x200 pixels for width and height and really poor palletes of around 16 colors. That allowed them to fit into smaller RAM and still had some reasonable graphic within typical NTSC/PAL CRT TV sets.

For example, Gameboy Advance had resolution of 240x160 (smallest reasonable numbers for vertical pixels), Nintendo NES used 256x240 (largest vertical numbers between all consoles) and Commodore64 had 320x200 (wide screen).

My first priority was and now to achieve the same nostalgic pixelate look of classic 8-bit games but target much bigger modern FullHD / 4K LED panels. So I kinda like to have a common integer divider between my resolution and 1920x1080. It will not hurt if I'll be able to use popular small LCDs too, which usually built around numbers like 240x320 or 640x480. And it always great if resolution will be dividable by 8 too.

After some draft calculations I start thinking about which will suite me best:

- [A] 320x180 - 1/6th of FullHD, divides by 8 and 10
- [B] 384x216 - 1/5th of FullHD or 1/10th of 4K, divides by 8 only
- [C] 480x270 - 1/4th of FullHD, divides by 10 only

To be honest, I really like the A variant most, but afraid it will be too small to implement good looking game. And C might be too big to work with and not worth it. So I'm going to use 384x216 for now.

Some reading on how to choose resolution:

https://rpgmaker.net/games/11197/blog/22259/

https://www.braveatnight.co.uk/post/5-tips-on-how-to-start-your-own-pixel-art

https://twitter.com/DavitMasia/status/1004742082752401408

https://twitter.com/davitmasia/status/1050129834968518657

# Audio

I like the sounds of Atari POKEY system. So for now I'd like to use something like that as blueprint for my audio subsystem.

# Programming Languages and Frameworks

After some research, I settled on Raylib and SDL2 libraries for graphic and GUI. Both are written in C99 so just to use the same language for the rest of platform looks like reasonable idea.

But after so many years of using high-level languages with garbage collector, proper strings handling, UTF8 support, networking and multi-threading, it felt like pain in the ass, so I decided to give a chance for some newer programming language with great C interop.

Some candidates were:

- [0] Pure C - best of the best, as most of low-level gamedev libraries for graphics and audio and even popular 6502 emulators are still written in that bespoken language. It just me who do not feel comfortable working with all the gotchas of manual memory management, string handling, etc.
- [1] Golang - robust, fast, well-known by me, reasonably good interop with with C. There also [1+] TinyGo which support all major platforms as well ESP32 and Arduino microcontrollers and have much-much faster FFI which is nice!
- [2] Odin is really great! Syntax looks very similar to Go. It much better for games development and has a better FFI. I'm just worried about tooling, IDE support and debugging, cause it not so popular as other candidates here. Still, that is my second best candidate after Golang.
- [3] Zig which cross-platform building system and C FFI really shines. But there no strings! Clumsy syntax. And annoying memory allocators everywhere. Looks like much harder target for me.
- [4] Vlang looks easy and cool on my eyes, similar to Odin. But there so many negative PR within Hacker News for the previous years and seems, it still uses C as an intermediate language and compiles with TinyCC which distracts me.
