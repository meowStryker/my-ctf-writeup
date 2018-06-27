# UniKLCTF_2018: Is it the same?

**Catgeory:** Cryptography
**Points:** xx
**Description:**

>.......

**Hint:**

>.......

## Write-up
When we looked at the file extension format in it's name, we could guess that something isn't right about it. Therefore, first what we must do is to confirm the actual format of the file.

Open the terminal & navigate to the location of the "tw3._uni.exe.jpg.pdf.png.apk" file, or just right click it & choose open terminal (if any) on the file navigator application (such as nautilus).

First, we run this command to check the actual format of the file

	file tw3_uni.exe.jpg.pdf.png.apk

The output will be like this

	tw3_uni.exe.jpg.pdf.png.apk: JPEG image data, JFIF standard 1.01, resolution (DPI), density 72x72, segment length 16, Exif Standard: [TIFF image data, big-endian, direntries=12, height=900, bps=0, PhotometricIntepretation=RGB, orientation=upper-left, width=945], baseline, precision 8, 800x762, frames 3

Or like this if you are using "-i" parameter (print mime type string) in the command which is much more simpler.

	tw3_uni.exe.jpg.pdf.png.apk: image/jpeg; charset=binary 

From the output,it tell us that the format of the file is actually ".JPEG" file. Next, once we already know the format of the file, we need to rename it so that the last name of the file would ended up as ".JPEG". Rename the file.

Using terminal in Linux

	mv tw3_uni.exe.jpg.pdf.png.apk tw3_uni.jpeg

Okay, now try to open the file to see if we are able to open it. In this case, we manage to open the image file successfully which means that the file is not corrupted. Since we are dealing with the steganography challenge, there are many ways of for the flag to be hidden. Usually, we will start with the easiest way to check for any possibility for data hiding in the file.


One of the easiest way to hide the data within an image file is by using meta-data. Meta-data is a data that describe about the data in the file.
Let us try using exiftools command to see any flag is hidden in the meta-data of the file.

	exiftools -a tw3_uni.exe.jpg.pdf.png.apk

After we analyze it, it seems that no flag is hidden in the meta-data. Therefore, we need to see for any printable strings within the image file if it is hidden using that method.
Use this command

	strings tw3_uni.exe.jpg.pdf.png.apk

it will give us a very long output printed line by line and somehow it last 3 lines are like this..

	q[6T
	c*fS
	https://pastebin.com/yyEYvSAG

The url inside an image file? Hurm.. its very fishy. We got this base64 value when we go to that link.

    ZFc1cGEyeGpkR1lvZVRCMVgzQnNOSGt6WkY5VVZ6TmZlVE4wUHlrPQ==

Nice. Just decode the base64 value 2 times using online tools or using command line to get the flag. 

	$ echo "ZFc1cGEyeGpkR1lvZVRCMVgzQnNOSGt6WkY5VVZ6TmZlVE4wUHlrPQ==" | base64 -d | base64 -d

The flag is "uniklctf(y0u_pl4y3d_TW3_y3t?)"
