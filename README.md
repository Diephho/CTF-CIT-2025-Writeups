# CTF@CIT 2025 writeups
![image](https://github.com/user-attachments/assets/f321e2e2-64c4-487c-a02e-156df343947f)

# Crypto
## Rotten
![image](https://hackmd.io/_uploads/ryteeGoJgl.png)

Using ROT13 cipher to decode this: `PVG{LxxdJwAXJGcsDoncKfRctddA}`

![image](https://hackmd.io/_uploads/HJkdlMokxx.png)

**FLAG: CIT{YkkqWjNKWTpfQbapXsEpgqqN}**

# Forensics
![image](https://hackmd.io/_uploads/rk84gQsylg.png)
## Brainrot Quiz!
![Screenshot 2025-04-26 222832](https://hackmd.io/_uploads/ryt6gfsyll.png)

Resource: [brainrot.pcap](https://github.com/Diephho/CTF-CIT-2025-Writeups/blob/main/Forensics/brainrot.pcap)

![image](https://hackmd.io/_uploads/ryZmbzokxe.png)

Use Wireshark, it has some ICMP packet. Tracing it and catching the packet No.11 that has the base64 message in Data

![image](https://hackmd.io/_uploads/B1DFZMskgl.png)

Decode base64 and get the flag

![image](https://hackmd.io/_uploads/HyFRZfjyxg.png)

**FLAG: CIT{tr4l4l3r0_tr4l4l4}**

## True CTF Love
![Screenshot 2025-04-26 222838](https://hackmd.io/_uploads/ryx-MGiJeg.png)

Resource: [The_Flag_Well_Capture_Together.eml](https://github.com/Diephho/CTF-CIT-2025-Writeups/blob/main/Forensics/The_Flag_Well_Capture_Together.eml)

This is an email forensics, to open .eml file, I using this web [EML Analyzer](https://analyzer.sublime.security/)
![Screenshot 2025-04-26 163943](https://hackmd.io/_uploads/rJgKMfoJxe.png)
Flag is hidden in `DKIM-signature`. This has 2 field `b=` - First is encrypt signature, second is base64 of the flag. Decode base64 and get the flag
![Screenshot 2025-04-26 164021](https://hackmd.io/_uploads/SJEIXGoygl.png)
**FLAG: CIT{i+l0v3_ctf$_t00}**

## We lost the flag
![image](https://hackmd.io/_uploads/Syws7GoJgg.png)

Resource: [lost.png](https://github.com/Diephho/CTF-CIT-2025-Writeups/blob/main/Forensics/lost.png)

This file is corrupted when opened

![image](https://hackmd.io/_uploads/rynr4fiyle.png)

Check with HxD, I see that has some hex `JFIF` - this byte is only appearing in JPEG file, and the file signature is corrupted. So my idea is changing the file signature to `JPEG`

![image](https://hackmd.io/_uploads/SJxiGHfjyex.png)

Check JPEG file signature with this [List of file signatures](https://en.wikipedia.org/wiki/List_of_file_signatures), fixing it, changing file type to .jpeg, opening it and getting the flag

![image](https://hackmd.io/_uploads/S1_UHMoygl.png)
![image](https://hackmd.io/_uploads/Hksf8Gsyee.png)

**FLAG: CIT{us1ng_m4g1c_1t_s33m5}**

## Bits 'n Pieces
![image](https://hackmd.io/_uploads/H1i0Fzjkeg.png)

Resource: [Cache0000.bin](https://github.com/Diephho/CTF-CIT-2025-Writeups/blob/main/Forensics/Cache0000.zip)

It's a .bin, so first I use HxD to check some bit in header

![Screenshot 2025-04-26 165909](https://hackmd.io/_uploads/Byk7czj1gg.png)

`RDP8bmp` is RDP bitmap cache. Use [mbc-tools](https://github.com/ANSSI-FR/bmc-tools) to extract bmp file, using option -b to combine all bmp file

```
python3 bmc-tools.py -s Cache0000.bin -d . -b
```
Open the `Cache0000.bin_collage.bmp`

![Screenshot 2025-04-26 170951](https://hackmd.io/_uploads/B1AKcfsygl.png)

Flag is in the pic.

**FLAG: CIT{c4ch3_m3_if_y0u_c4n}**

## Baller
![image](https://hackmd.io/_uploads/HJy5jzi1gl.png)

Resource: [baller.zip](https://github.com/Diephho/CTF-CIT-2025-Writeups/blob/main/Forensics/baller.zip)

When I tried to unzip it, I got this Warning. I thought wrong file extension/wrong bit or mistake structure causes this problem.
![image](https://hackmd.io/_uploads/HkaghMjkel.png)

Check with HxD, I saw file name `01.txt` so it is real zip file, no mistake with file signature.
![Screenshot 2025-04-26 103528](https://hackmd.io/_uploads/rJaw6zskle.png)

To check the hidden files, I used `binwalk` and saw that there were 4 zipped files: `01.txt`, `02.txt`, `03.txt` and a GIF image

![image](https://hackmd.io/_uploads/r1Fv1Xskxl.png)


Extract with binwalk option -e, but text in those .txt file is not include flag. The GIF image is not extracted with binwalk, so I use `dd` to extract it.

```
dd if=baller.zip of=hidden.gif bs=1 skip=16631
```

Open GIF image 

![image](https://hackmd.io/_uploads/SJXskmo1ex.png)

The flag is in the lower right corner

**FLAG: CIT{im_balling_fr}**

# Steganography
![image](https://hackmd.io/_uploads/B14XlXjylg.png)
## Blank Image
![image](https://hackmd.io/_uploads/SkkflQi1eg.png)

Resource: [image.png](https://github.com/Diephho/CTF-CIT-2025-Writeups/blob/main/Steganography/image.png)

This image has size 8x17, so it's hard to see with eyes.
Check with `strings` for content and `zsteg` for lsb, I got the flag.

![Screenshot 2025-04-26 171954](https://hackmd.io/_uploads/SyBTxQjJle.png)

**FLAG: CIT{n1F0Rsm0Er40}**

## I AM Steve
![image](https://hackmd.io/_uploads/SyJWWQokel.png)

Resource: [ChickenJockey.png](https://github.com/Diephho/CTF-CIT-2025-Writeups/blob/main/Steganography/ChickenJockey.png)

![ChickenJockey](https://hackmd.io/_uploads/r1UhbQj1xg.png)

I saw that it has a mini black line in the top of the image, so maybe something was hidden in color bit.

About color bit, using `zsteg` to extract, I got a base64 in `b1,rgb,lsb,xy`

![image](https://hackmd.io/_uploads/rJ7UGmsyxx.png)

Decode it and get the flag

![image](https://hackmd.io/_uploads/SyPPf7s1lg.png)

**FLAG: CIT{THIS_is_a_crafting_table}**

## sw0906
![image](https://hackmd.io/_uploads/SJf5fQoJgg.png)

Resource: [yoda](https://github.com/Diephho/CTF-CIT-2025-Writeups/blob/main/Steganography/yoda)

It is a data file, first check with HxD
![Screenshot 2025-04-26 173517](https://hackmd.io/_uploads/By0aMmjklg.png)

I see something familier. Check with [List of file signatures](https://en.wikipedia.org/wiki/List_of_file_signatures), with first 4 bytes, It looks like JPEG but in reverse.
JPEG starts with `FF D8 FF E0` `00 10 4A 46` `49 46 00 01`
But this starts with `E0 FF D8 FF` and next `46 4A 10 00`

I fixed those bytes but the image was still corrupted. 
Finally, I got it, not only magic bytes but also all bytes of file, with 4 consecutive bytes, it is written in reverse. Write a python program to repair it

```python
def fix_reverse_blocks(input_path, output_path):
    with open(input_path, "rb") as f:
        data = f.read()

    fixed_data = bytearray()

    # Xử lý từng block 4 byte
    for i in range(0, len(data), 4):
        block = data[i:i+4]
        fixed_data.extend(block[::-1])  # đảo ngược block

    with open(output_path, "wb") as f:
        f.write(fixed_data)

    print(f"Đã ghi file đã sửa vào: {output_path}")

# Ví dụ dùng
fix_reverse_blocks("yoda", "output_yoda.jpeg")
```

Open fix file and get the flag

![image](https://hackmd.io/_uploads/rysKNmjyge.png)

**FLAG: CIT{h1dd3n_n0_m0r3_1t_i5}**

## Sorry, you're NOT a sigma
![image](https://hackmd.io/_uploads/SkLVrms1ge.png)

Resource: [lion.mp4](https://github.com/Diephho/CTF-CIT-2025-Writeups/blob/main/Steganography/lion.mp4)

The describe give the hint "track" so I just follow it.

Use `ffmpeg` to show all track in mp4, I use [ffmpeg online](https://ffmpeg-online.vercel.app/?inputOptions=-i&output=output.mp4&outputOptions=)
```
ffmpeg -i lion.mp4
```
This show all streams (track) in the file
![Screenshot 2025-04-26 174751](https://hackmd.io/_uploads/Sk8yUXjkex.png)

There are 3 streams (#0:0, #0:1 and #0:2)
- Stream 0:0 - Video - It seems normal video
- Stream 0:1 - Audio (48kHz Stereo) - Default audio
- Stream 0:2 - Audio (22kHz Stereo) - Sus!!!, 22050 Hz is low rate to hide info

Extract this track with ffmpeg
```
ffmpeg -i lion.mp4 -map 0:2 -c copy hidden_audio.aac
```
Covert to `.wav` for analysis
```
ffmpeg -i hidden_audio.aac hidden_audio.wav
```
Open with `Audacity` and use mode `Spectrogram`

![Screenshot 2025-04-26 175141](https://hackmd.io/_uploads/Hkg-_7okle.jpg)

Get an image about command. Run this command and get the flag

![Screenshot 2025-04-26 175710](https://hackmd.io/_uploads/ByuX_msJex.png)

**FLAG: CIT{wh3n_th3_l10n_sp34k5_y0u_l1st3n}**

## Queen's Gambit

![image](https://hackmd.io/_uploads/S1ly0QnJxx.png)

Use zsteg to extract lsb, get the chess move

![image](https://hackmd.io/_uploads/Hyy2am3kee.png)

Put it in chess board, I see the word "PWN"

![Screenshot 2025-04-28 054738](https://github.com/user-attachments/assets/e90e2b21-5a09-4514-ab7f-aff46d1dfb26)

**FLAG: CIT{PWN}**

# MISC
## Robots
![image](https://hackmd.io/_uploads/BkauuQjklg.png)

Check /robots.txt in url

![image](https://hackmd.io/_uploads/SyBp_Xoygg.png)

**FLAG: CIT{m6F2nr8RgjYI}**

## Calculator
![image](https://hackmd.io/_uploads/H1vxF7oyxg.png)

Resource: [calculator.lua](https://github.com/Diephho/CTF-CIT-2025-Writeups/blob/main/Misc/calculator.lua)

![image](https://hackmd.io/_uploads/S1A4KQjJll.png)

This code is just to trick players.

At the end of the file, finding something maybe is the main of this challenge

![image](https://hackmd.io/_uploads/Hy92Fmi1gg.png)

It looks like `Whitespace language`, so I use [dcode](https://www.dcode.fr/whitespace-language) to decode it

![image](https://hackmd.io/_uploads/rkA-qXo1gg.png)

**FLAG: CIT{hft4bT0415Lb}**

## Select all squares that contain uhh...
![image](https://hackmd.io/_uploads/ryUS9Xo1lg.png)

Follow that link, I get a website when I click on reCAPTCHA, it creates a powershell command in my clipboard

That command is so suss!!, it is an obfuscation powershell that seem run something bad in my PC, but I trust the author =)) so just run it (~~I run it in virtual machine~~)

![Screenshot 2025-04-26 221434](https://hackmd.io/_uploads/HJp7oXj1eg.png)

Deobfuscation that command is quite hard for me, so I check Windows Event about file creation and open some directories I think it could be found.

It is in `Local/Temp/`

![image](https://hackmd.io/_uploads/r13S2Xo1xe.png)

**FLAG: CIT{th1s_a1nt_m4lw4r3_d0nt_w0rry}**

# OSINT
![image](https://hackmd.io/_uploads/ryLjnmikle.png)
## No Country for Old Keys
![image](https://hackmd.io/_uploads/SJVp3Xsylg.png)

Searching and finding there are two media: `linkedin and github`. About API key, check github

![image](https://hackmd.io/_uploads/SJ24aXi1xl.png)

It has only one project, check it's commits (there are 7 commits)

![image](https://hackmd.io/_uploads/SyV3pQjygx.png)

Check `removed my API key` commit and get it

![image](https://hackmd.io/_uploads/r1XAT7iJll.png)

**FLAG: CIT{ap9gt04qtxcqfin9}**

## The Domain Always Resolves Twice
![image](https://hackmd.io/_uploads/BkDbCQjkee.png)

The github has no more information, go to visit the linkedin

![image](https://hackmd.io/_uploads/HJvFC7iyeg.png)

He has a post a about website and domain. 
"And here's a fun fact – he even registered his domain with my favorite registrar! 😎 This guy… dare I say it... ROCKS!"

Let's check this domain with [Whois](https://who.is/)

![image](https://hackmd.io/_uploads/BJBkyVo1ex.png)

**FLAG: GIT{GoDaddy.com}**

## Throwback to the Future
![image](https://hackmd.io/_uploads/HJO71ViJle.png)

There no more information in Linkedin, next searching with username found in github (antmcconn)

![image](https://hackmd.io/_uploads/rJyOJVjkex.png)

Found an X account [antmcconn](https://x.com/antmcconn)

![image](https://hackmd.io/_uploads/S1D6JVikge.png)

See a post with hashtag `#throwback`, so we need to check the day of this event.
Search with Google Lens

![image](https://hackmd.io/_uploads/H11Hg4okgl.png)

It is in Gillette Stadium

Now, using the power of Artificial Intelligence. :fire: 

![image](https://hackmd.io/_uploads/BJ9olVi1le.png)

**FLAG: CIT{10/22/2023}**

## Timesink
![image](https://hackmd.io/_uploads/H1pgOrjkxe.png)

Searching using Google Lens with that brigde

![Screenshot 2025-04-27 130037](https://hackmd.io/_uploads/ByIS_Sjyge.jpg)

![image](https://hackmd.io/_uploads/Byhv_royeg.png)

It is "Little Nestucca River Bridge II"

![Screenshot 2025-04-27 130244](https://hackmd.io/_uploads/r1e3uroylx.jpg)

The road name is the flag

**FLAG: CIT{Little_Nestucca_River_Rd}**

# Reverse Engineering
## Read Only
![image](https://hackmd.io/_uploads/rkVX-4iJex.png)

Resource: [readonly](https://github.com/Diephho/CTF-CIT-2025-Writeups/blob/main/Rev/readonly)

Use `IDA` to open it
Check the `start function`
![image](https://hackmd.io/_uploads/SkldGViJgg.png)

It call to `sub_407C05 function`, so I follow that.
![image](https://hackmd.io/_uploads/SkJsGVoJel.png)

The flag is show through v6, and v6 reads the string CIT{87z1BjG1968G} so It is the flag.

**FLAG: CIT{87z1BjG1968G}**
