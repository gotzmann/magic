# Magic X

I always dreamed of kind of project involving custom hardware and low-level software development. And it looks like your very own Game Console might be the coolest thing to do in the realm, right?

So a year ago I've started looking around and tinkering with some reborn nostalgia consoles like SNES Classic Mini and Analogue Sega built on FPGA as well completely new beasts like virtual console PICO-8.

# CPU

There were two popular CPUs gathered iconic status over the years: MOS 6502 and Zilog Z80. 

The first was used in 8-bit Atari line, Apple computers, Commodore 64, Nintendo NES and many others.

The second went as ZX Spectrum, Amstrad family and some others, popular within Japan and Europe.

I used to own and use many 6502-based systems including Soviet clone of Apple ][, Asian replic of NES and real Ataris and Commodores.

So after some research on history I've decided to use 6502 as my target for Magic brains.

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

- Golang as it fast, reasonably good when dealing with C. There also TinyGo which works on ESP32 and Arduino and have much-much faster FFI which is nice!
- Zig which cross-platform building system and C FFI really shines. But there no strings! Clummsy syntax. And annoying memory allocators everywhere.
- Odin is really great! Syntax looks very similar to Go. It much better for games development and has a better FFI. So that my second best candidate after Golang.
- Vlang looks good. But there so many negative PR within Hacker News for the previous years and it seems, it still uses C as an intermediate language and compiles with TinyCC which distracts me.
-  
