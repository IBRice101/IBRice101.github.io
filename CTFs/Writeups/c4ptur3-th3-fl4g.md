# CTF Walkthrough — c4ptur3-th3-fl4g — tryhackme.com

(originally published Oct 17 2019)

![The chart at the beginning of the CTF](media/THMgraph.png)

This is the first in my series of “Capture The Flag” walkthroughs for [tryhackme](https://www.tryhackme.com/), and as such I feel as if It’s probably best to start with a fairly easy CTF, that is [c4ptur3-th3-fl4g: A beginner level CTF challenge](https://tryhackme.com/room/c4ptur3th3fl4g), by dcdavidlee. This CTF Covers a few of the basics of what to expect in a CTF and is an excellent foundation for someone to build their skills over time.

This CTF has 5 Tasks, these are Translation & Shifting, Hashes, Spectrograms, Steganography, and Security through Obscurity.

## 1. Translation & Shifting

This section requires the player to perform a number of fairly standard translations and cipher solves in order to obtain the flag, including but not limited to Binary and Hexadecimal to ASCII, as well as Morse, and, as we will see in a second, the hacker’s CLASSIC… 13375p34k

### Question 1: c4n y0u c4p7u23 7h3 f149?

13375p34k is, as I’m sure you’ll know, “a system of modified spellings used primarily on the Internet”, where some letters are replaced by numbers and other spellings (as in hax0rz, pwned, and n00b). This is clearly in 1337, no question about it.

**Flag: can you capture the flag?**

### Question 2: 01101100 01100101 01110100 01110011 00100000 01110100 01110010 01111001 00100000 01110011 01101111 01101101 01100101 00100000 01100010 01101001 01101110 01100001 01110010 01111001 00100000 01101111 01110101 01110100 00100001

Does the series of 1’s and 0’s give it away? This is binary, the “language of computers” as journalists correctly, but all too frequently, point out for the benefit of those not terribly technologically inclined.

I made use of [RapidTables’s Binary to Text Translator](https://www.rapidtables.com/convert/number/binary-to-ascii.html) for this question, which worked a treat.

![Binary Translator](media/BinaryToText.png)

**Flag: lets try some binary out!**

### Question 3: MJQXGZJTGIQGS4ZAON2XAZLSEBRW63LNN5XCA2LOEBBVIRRHOM======

Okay, now it’s getting into slightly newer territory! This question deals with Base32 values, base 32 being a number system that is made up of the 26 letters of the English alphabet (A-Z) AS WELL AS the numbers 2–7, allowing for a total of 32 usable characters in each position.

There are several ways you’re able to tell apart Base32 from other bases, it’s definitely not lower than base 10 because there are letters involved, it’s not as high as Base64 because (as we will see), there are no lower-case characters and the numbers 0-1 and 8–9 aren't represented.

I used [Base32 Decode Online](https://emn178.github.io/online-tools/base32_decode.html).

![Base 32 Decoder](media/Base32Decode.png)

**Flag: base32 is super common in CTF’s**

### Question 4: RWFjaCBCYXNlNjQgZGlnaXQgcmVwcmVzZW50cyBleGFjdGx5IDYgYml0cyBvZiBkYXRhLg==

This is in Base64, you can tell because lower case characters are included this time, pretty cut and dry.

I used [Base64 Decode and Encode](https://www.base64decode.org/).

![Base 64 Decoder](media/Base64Decode.png)

**Flag: Each Base64 digit represents exactly 6 bits of data**

### Question 5: 68 65 78 61 64 65 63 69 6d 61 6c 20 6f 72 20 62 61 73 65 31 36 3f

This is in Hexadecimal, or Base16. Each character is always 1 byte, or 8 bits long, helpfully each hexadecimal number is 4 bits, meaning 2 hex digits can ALWAYS represent a character in ASCII.

The way you can tell it’s in hex is the spacing between each couplet and the character set, that is 0–9 then a-f, adding to 16.

I used [RapidTables](https://www.rapidtables.com/convert/number/hex-to-ascii.html) again for this conversion.

![Hex to ASCII converter](media/HexToAscii.png)

**Flag: hexadecimal or base16?**

### Question 6: Ebgngr zr 13 cynprf!

ROT13 is an extremely simple cipher where each letter in a phrase is “rotated” 13 times, so that a letter becomes the letter 13 places after it, for example A <-> N, I <-> V, and so on (my name is Vfnnp, as an extra example you didn't ask for.) Rot13 is its own inverse because 13 is half 26, there are 26 letters in the alphabet, meaning that the letter A will translate to N and the letter N will translate back to A, making the cipher trivial to break.

To speed things up, I used [rot13.com](https://rot13.com/).

![Rot 13 Cipher](media/Rot13.png)

**Flag: Rotate me 13 places!**

### Question 7: \*@F DA:? >6 C:89E C@F?5 323J C:89E C@F?5 Wcf E:>6DX

This is a Rot47 cipher, it’s the same basic principle as the Rot13 but instead of using just the letters A-Z, it uses all characters in the ASCII encoding table, A-Z, 0–9, punctuation, and so on.

I used [dcode’s decoder](https://www.dcode.fr/rot-47-cipher) for this cipher

![Rot 47 Cipher](media/Rot47Cipher.png)

**Flag: You spin me right round baby right round (47 times)**

### Question 8: — . .-.. . -.-. — — — — ..- -. .. -.-. .- — .. — — -. . -. -.-. — — -.. .. -. — .

It’s Morse code, Dots and dashes, you know? Just obvious I suppose, not much more else to say here

I used [browserling’s Morse code decoder](https://www.browserling.com/tools/morse-to-text)

![Morse Code to Text Converter](media/MorseToText.png)

**Flag: telecommunication encoding**

### Question 9: 85 110 112 97 99 107 32 116 104 105 115 32 66 67 68

This is much the same as the previous translation and decoding questions (using hex and base64 and whatnot) but instead this time we’re using good ol’ reliable base 10, 0–9, what’s known as BCD, or Binary Coded Decimal, where each number represents its value in binary, which is then converted to ASCII and spat out as a series of letters and numbers. Regardless, Base10 is what we’re used to, great stuff.

I returned to [Cryptii’s](https://cryptii.com/pipes/text-decimal) decoder for this task.

![Unicode code points](https://cryptii.com/pipes/text-decimal)

**Flag: Unpack this BCD**

### Question 10: LS0tLS0gLi0tLS0gLi0tLS0gLS0tLS0gLS0tLS0gLi0tLS0gLi0tLS0gLS0tLS0KLS0t……….

Oh boy…

This one was a long winded one, so I’ll spare the screenshots and the links to decoders and what have you.

This question was designed to make sure you were paying attention, the original message was in Base 64, as one can tell from the mixture of upper and lower case characters. This decoded into Morse, which itself decoded into Binary, which then decoded into a ROT47 cipher, which THEN decoded to decimal, and FINALLY spat out…

**Flag: Let’s make this a bit trickier…**

Someone’s having a giggle…

## 2. Hashes

The author recommends brute forcing these hashes through hashcat, however the constraints of my system means this will take over a day. Websites such as [md5decrypt](https://md5decrypt.net/en/) work especially well for this as the hashes should already be stored in their databases. In CTFs you take what you can, I suppose.

### Question 1: 39d4a2ba07e44421c9bedd54dc4e1182

Once I resolved to use md5decrypt I realised that whilst this section was still one of the longest, it was just a matter of finding what hash was used for each flag and using that to crack it.

This hash had the hint “This method of encryption is no longer considered ‘secure’. It’s an MD, but which one?”, which told me that it was encoded using Message Digest 2, the simplest (read: most insecure) in the series of MD hashes that provide 128-bit values

**Flag: MDwhat?**

### Question 2: e0418e7c6c2f630c71b2acabbcf8a2fb

The hint here is “Better than MD2, but not as good as MD5”, which means it’s using MD4.

**Flag: digest the message algorithm**

### Question 3: efbd448a935421a54dda43da43a701e1

This question didn't have a hint, however, I reasoned that seen as how the last two hashes were both MD and went up in difficulty to crack, logically this next hash must be MD5, I was correct

**Flag: 128-bit of delicious hash values**

### Question 4: 11FE61CE0639AC2A1E815D62D7DEEC53

The hint here is “SoftMicro”, Which fairly transparently is a reference to Microsoft. After a bit of research I discovered that Microsoft has a somewhat proprietary hashing algorithm called NTLM, or New Technology (Microsoft’s Kernel) LAN Manager.

**Flag: Microsoft has encryption**?

### Question 5: a361f05487b879f25cc4d7d7fae3c7442e7849ed15c94010b389faafaf8763f0dd022e52364027283d55dcb10974b09e7937f901584c092da65a14d1aa8dc4d8

The hint was “My heart goes SHAlalalala SHA lala 512 times!”. The capitalisation of the word SHA plus the mention of the number 512 means that this is a SHA512 Hash, (SHA for Secure Hashing Algorithm, and 512 for the digest, or hash value size in bits).

**Flag: 1024 bit blocks!**

### Question 6: d48a2f790f7294a4ecbac10b99a1a4271cdc67fff7246a314297f2bca2aaa71f

There was no hint here, once again. I thought, however, maybe seen as how the last hash was a SHA512, maybe this is a SHA hash of half the digest of the previous one? This was correct, so I used the SHA256 Decryption algorithm on it.

**Flag: Commonly used in Blockchain**

### Question 7: a34e50c78f67d3ec5d0479cde1406c6f82ff6cd0

The hint here was “The First SHA”, Must be SHA-1

**Flag: The OG**

## 3. Spectrograms

According to Wikipedia, A spectrogram is “a visual representation of the spectrum of frequencies of a signal as it varies with time. When applied to an audio signal, spectrograms are sometimes called sonographs, voiceprints, or voicegrams. When the data is represented in a 3D plot they may be called waterfalls.”

### Question 1: secretaudio.wav — download the file

The hint here was “audacity”.

To do this question I recommend already having installed an audio editing program, my personal recommendation (as well as the recommendation of the author) is Audacity. You can install it off the website or, if you are so inclined, use the command:

```sh
sudo apt-get install audacity
```

Once installed, open the file given to you (called secretaudio.wav) and change the view from waveform to spectrogram in the menu with the downwards facing black triangle off to the side, like so:

![Steganographic output in Audacity](media/Audacity.png)

As you can see, it now displays the flag

**Flag: Super Secret Message**

## 4. Steganography

Steganography is “The process of hiding a message or file within another message or file”

### Question 1: stegosteg.jpg — Decode the image to reveal the answer.

I used [futureboy.us’s steganography decoder](https://futureboy.us/stegano/decinput.html) on the image, as you can see below:

![The original image (stegosteg.jpg)](media/Stegosteg.png)
![The steg decoder](media/StegDecoder.png)

**Flag: SpaghettiSteg**

## 5. Security through Obscurity

Finally, I solved this section in a manner that I don’t believe was intended, but hey, that's what hacking is isn't it?.

I opened up the file in a text editor, scrolled to the bottom, and found the two flags in plaintext

**Flag1: hackerchat.png**

**Flag2: AHH_YOU_FOUND_ME!**

## Conclusion

This CTF Is pretty good, and works an absolute treat for beginners, I’d recommend it to my nan, if she ever wanted to become a 1337 h4x0r.

If you learned something from this, why not send me over a little tip by way of thanks? No pressure but it would be much appreciated :)

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/A0A1D0FSN)