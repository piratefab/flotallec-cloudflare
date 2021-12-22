---
layout: post

title: Speakable Public Identities
category: blog

author:
  name: Chris Ballinger
  twitter: chrisballingr
  gplus: 110173710196322914492 
  bio: Founder & Lead Developer
---

# Speakable Public Identities

I've been doing a lot of thinking about novel ways to represent 32-byte `Ed25519` public identity keys vocally, visually, and otherwise. Here's a simplified version (`v0.1`) for the sake of example.

If we choose an octal set of 8 emoji characters with a parallel prounounceable syllabylic mapping, you can represent a 32-byte public key (256-bits) in X characters.

### Octal / Decimal / Hexidecimal	/	Emoji	/	Syllable Mapping

| Dec  | Oct    |  Hex   | Emoji | Syllable |
|------|--------|--------|-------|----------|
| `0`	| `0o00` | `0x00` | 🌞	| ohm	   |
| `1`	| `0o01` | `0x01` |	🌵	| ma 		| 
| `2`	| `0o02` | `0x02` |	🌲	| ni 		| 
| `3`	| `0o03` | `0x03` |	🌼	| pad 		| 
| `4`	| `0o04` | `0x04` |	🐅	| me 		| 
| `5`	| `0o05` | `0x05` |	🕊	| hum 		| 
| `6`	| `0o06` | `0x06` |	🐉	| fre 		| 
| `7`	| `0o07` | `0x07` |	🌅	| dom 		| 
| `8`	| `0o10` | `0x08` |	🌉	| mod 		| 
| `9`	| `0o11` | `0x09` |	🌌	| erf 		| 
| `10`	| `0o12` | `0x0A` |	🌋	| muh 		| 
| `11`	| `0o13` | `0x0B` |	🕸	| em 		| 
| `12`	| `0o14` | `0x0C` |	🐚	| dap 		| 
| `13`	| `0o15` | `0x0D` |	⭐️	| in 		| 
| `14`	| `0o16` | `0x0E` |	☄	| am 		| 
| `15`	| `0o17` | `0x0F` |	💎	| mho 		|



Here's a 256-bit (32-byte) random octal number from [`random.org`](https://www.random.org/cgi-bin/randbyte?nbytes=32&format=o), which is the same length as an Ed25519 signing key:

> Octal (triples): 012 000 253 040 076 137 346 101 323
>  323 250 007 215 234 235 101 335 245 015 021 101 064
>  267 373 301 126 013 356 317 120 123 026
> 
> Octal: 01200025304007613734610132332325000721523
> 423510133524501502110106426737330112601335631712
> 0123026
>
> Decimal (truncated): 9.713978307904121E+84
> 
> Hexadecimal (truncated): 500156201F17DC0000000000000000000
> 0000000000000000000000000000000
> 0000000
>  
> Binary (truncated): 101000000000001010101100010000000
> 01111100010111110111000000000000
> 0000000000000000000000000000000000
> 000000000000000000000000000000000000
> 00000000000000000000000000000000000000
> 0000000000000000000000000000000000000000
> 00000000000000000000000000000000000000000
> 00000000000000000000000000000

To convert this to our new mapping, simply replace all of the numbers with their equivalent emoji character, or pronounceable syllable. Obviously mostly humans don't have the patience to whisper out loud the whole key, but even speaking the beginning `N`(?) characters is good enough to prove your identity cryptographically. To bruteforce past the last `N` characters would take an unreasonable amount of time, without the availability of quantum computers of course.

If you run this random octal UTF-8 string through the `ears_v01()` function, you get the following result:

> ohmmani ohmohmohm nihumpad ohmmeohm ohmdomfree mapaddom padmefree maohmma padnipad padnipad nihumohm ohmohmdom nimahum nipadme nipadhum maohmma padpadhum nimehum ohmmahum ohmnima maohmma ohmfreeme nifreedom paddompad padohmma manifree ohmmapad padhumfree padmadom maniohm manipad ohmnifree

You can also see the emoji version below:

> 🌞🌵🌲 🌞🌞🌞 🌲🕊🌼 🌞🐅🌞 🌞🌅🐉 🌵🌼🌅
>  🌼🐅🐉 🌵🌞🌵 🌼🌲🌼 🌼🌲🌼 🌲🕊🌞 🌞🌞🌅
>  🌲🌵🕊 🌲🌼🐅 🌲🌼🕊 🌵🌞🌵 🌼🌼🕊 🌲🐅🕊
>  🌞🌵🕊 🌞🌲🌵 🌵🌞🌵 🌞🐉🐅 🌲🐉🌅 🌼🌅🌼
>  🌼🌞🌵 🌵🌲🐉 🌞🌵🌼 🌼🕊🐉 🌼🌵🌅 🌵🌲🌞
>  🌵🌲🌼 🌞🌲🐉

If we choose 10 or 16 syllables and emojis, we can reduce the length of the key by using decimal or hexadecimal mapping instead of octal. 

Have any ideas for other novel ways to represent this information? Smells? Music? Dances? The sky's the limit!

### Sample Code

Download [`speakkey.py`](/code/speakkey.py).

```python
#! /usr/bin/python3.5
# Copyright 2015 Chris Ballinger - CC0 1.0 Universal

import sys

# Octal / Emoji / Syllable Mapping
# `0	|	🌞	| ohm`
# `1	|	🌵	| ma`
# `2	|	🌲	| ni`
# `3	|	🌼	| pad`
# `4	|	🐅	| me`
# `5	|	🕊	| hum`
# `6	|	🐉	| free`
# `7	|	🌅	| dom`


# Input octal UTF-8 string (e.g. '012345670123456701234567') and receive
# an emoji representation.
def eyes_v01(o_string: str) -> str:
    o_string = o_string.replace('0', '🌞')
    o_string = o_string.replace('1', '🌵')
    o_string = o_string.replace('2', '🌲')
    o_string = o_string.replace('3', '🌼')
    o_string = o_string.replace('4', '🐅')
    o_string = o_string.replace('5', '🕊')
    o_string = o_string.replace('6', '🐉')
    o_string = o_string.replace('7', '🌅')
    return o_string


# Input octal UTF-8 string (e.g. '012345670123456701234567') and receive
# an pronounceable syllable representation.
def ears_v01(o_string: str) -> str:
    o_string = o_string.replace('0', 'ohm')
    o_string = o_string.replace('1', 'ma')
    o_string = o_string.replace('2', 'ni')
    o_string = o_string.replace('3', 'pad')
    o_string = o_string.replace('4', 'me')
    o_string = o_string.replace('5', 'hum')
    o_string = o_string.replace('6', 'free')
    o_string = o_string.replace('7', 'dom')
    return o_string

if __name__ == '__main__':
    if len(sys.argv) < 2:
        print('speakkey - v0.1\n\nUsage: speakkey.py "01234567 01234567 01234567" (octal only)')
    else:
        input_string = sys.argv[1]

        print('Eyes v0.1: ' + input_string)
        eyes = eyes_v01(input_string)
        print('         : ' + eyes)

        print('Ears v0.1: ' + input_string)
        ears = ears_v01(input_string)
        print('         : ' + ears)

```