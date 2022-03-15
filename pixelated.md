
# PicoCTF – pixelated

* **Category:** Cryptography
* **Points:** 100

## Challenge
I have these 2 images, can you make a flag out of them? [scrambled1.png](https://mercury.picoctf.net/static/c9593d1d2ac9d850da95bffe0ac3b6c6/scrambled1.png) [scrambled2.png](https://mercury.picoctf.net/static/c9593d1d2ac9d850da95bffe0ac3b6c6/scrambled2.png)

## Hints
1: [https://en.wikipedia.org/wiki/Visual_cryptography](https://en.wikipedia.org/wiki/Visual_cryptography)
2: Think of different ways you can "stack" images

## Solution
For this challenge, I first tried to open the two images in GIMP, and use different blend options to try and find the flag. Going through options such as difference, multiply, and addition, I didn't find any results. Since this wasn't working, I found a plugin called G'mic that can manipulate images. Unfortunately it wouldn't cooperate with my WSL linux, so I then went to pillow package in python.

In sublime, I ran 

<code>from PIL import Image, ImageChops                                                                                       
im1 = Image.open('scrambled1.png')                                                                                      
im2 = Image.open('scrambled2.png')                                                                                      
im3 = ImageChops.add(ImageChops.subtract(im2, im1), ImageChops.subtract(im1, im2))                                      
im3.show()                                                                                                             
im3.save("\lxtemp\scrambledOUT.png")</code>

(based on the code posted by Advin Saz here https://crypto.stackexchange.com/questions/88430/how-to-decrypt-two-images-encrypted-using-xor-with-the-same-key)

Unfortunately ended up with a similarly random image image, so it seems like XOR isn't the way to solve this problem.

I then tried adding the images with `im3 = ImageChops.add(im1, im2)`
The issue with that was that the pixels didn't loop back, instead keeping rgb values at 255, which results in an almost compleltely white image. To fix this, we need to find some way to let the values flow back to 0, instead of capping at 255. While mod% is a good way to do this, I instead did this:

<code>from PIL import Image, ImageChops      
import numpy as np                                                                                 
im1 = Image.open('scrambled1.png')                                                                                      
im2 = Image.open('scrambled2.png')  

px1 = np.array(im1)
px2 = np.array(im2)

added = px1 + px2     
im3 = Image.fromarray(added)                  
im3.show()                                                                                                             
im3.save("\lxtemp\scrambledOUT.png")</code>

by creating arrays out of our images, the max value can only be 255. That means when we add them together, any values over that overflow and start back at 0. When running this the image displays the flag clearly.

FLAG
`picoCTF{da8fcef8}`