```
██████╗  █████╗ ████████╗ █████╗     ██╗  ██╗ ██████╗ ██╗   ██╗███╗   ██╗██████╗ 
██╔══██╗██╔══██╗╚══██╔══╝██╔══██╗    ██║  ██║██╔═══██╗██║   ██║████╗  ██║██╔══██╗
██║  ██║███████║   ██║   ███████║    ███████║██║   ██║██║   ██║██╔██╗ ██║██║  ██║
██║  ██║██╔══██║   ██║   ██╔══██║    ██╔══██║██║   ██║██║   ██║██║╚██╗██║██║  ██║
██████╔╝██║  ██║   ██║   ██║  ██║    ██║  ██║╚██████╔╝╚██████╔╝██║ ╚████║██████╔╝
╚═════╝ ╚═╝  ╚═╝   ╚═╝   ╚═╝  ╚═╝    ╚═╝  ╚═╝ ╚═════╝  ╚═════╝ ╚═╝  ╚═══╝╚═════╝
``` 

# **What is Data Hound?**

DataHound is a toolkit that I created and developed for my final year project as a part of my BSc. It's written in Python 3 and aims to bridge the gap in current forensics tools when it comes to finding data hidden by both cryptographic and steganographic techniques.

# **Why?**

The inspiration came to me, one rainy afternoon - sitting in my study, wearing my fluffy bunny slippers, silk robe on, and drinking a goblet of coffee ... 

Just kidding (not about the slippers), but it DID come to me, whilst I was playing in a CTF tournament, one of the flags hidden within an image, only becoming visible after duplicating the image, and laying it over the top of the original image, with milimeters of offset.
And the thought struck me, are our current tools enough to find steganographically hidden items? 
What about encrypted and then hidden steganographically?
What about HIDDEN IN PLAIN SIGHT?!

My mind reeling.... off I went for some research.

# **The problems**

Oh boy, did I find some problems...
Firstly, in the realms of legal data aquisition regarding computer forensics... for the sake of speed, and cost, only the areas typically seen in previous cases are examined. 
In an article from the Interpol review of digital evidence 2016 – 2019: *"Most proposed methods for speeding up digital evidence examination are based on the assumption that relevant information will be found in similar locations where it has been found in other cases. Consequently, evidence stored in previously unknown or new locations will be ignored".*
I then read through some court cases and both UK and US legislations relating to data hiding techniques - the biggest problem legally, is that in order to evade self-incrimination, steganography is used for plausible deniability. In laymen's terms... you can't file a warrant, without knowing what you need it for.

# **Cue sample data and forensic testing**

First place to start, was with something simple. So I tried music files (yes, yes, I know most people stream these days - but remember the idea was hiding in plain sight... an album on a desktop wouldn't be questioned). I hid a number of files within a handful of wav files - encrypted and non encrypted using LSB steganography, to ensure the file sizes weren't changed in the process, with the 4 main file types being: docx, png, txt and 7zip. Then ran them through some of the most used tools in the computer forensics space, to see if any of them would pick up on the files.

The results were... er... unexpected.

None of the "gold standard" tools could identify steganography...
<p align="center">
<img align="middle" width="600" src="https://github.com/R3D1AM/Blog/blob/main/_resources/DataHound_RelatedTools.png"/>
</p>

and testing FTK imager and Autopsy, they didn't identify the wav files had been tampered with either.
<p align="center">
<img align="middle" width="600" src="https://github.com/R3D1AM/Blog/blob/main/_resources/DataHound_FTKFileTampering.png"/>
</p>

# **Found the hole, grab the polyfilla!**

So, I decided to try to create a toolkit that COULD identify stegangraphically altered file types (starting with wav files) with hopes for expansion further down the line. That alone would have been too easy of course * heavy sarcasm * so I decided to also add the functionality of extracting the hidden files, and decrypting them as necessary.

First thing is first, the toolkit requires the user to create a hash of the original, unedited files - or in this case album to verify file integrity. This can be done running the **create_md5_of_music.py** and then copying the output into the **db_creator.py script**, to attach the hashes to a database, for later analysis.
I know what you're thinking... BUT MD5?! for this case yes, simply for the sake  of speed. Although the **create_md5_of_music.py** script can easily be modified to create whichever hash type you prefer.

Now we're ready to run the main DataHound script, we placed the tampered with audio files in the **/original** folder and Data Hound will then make copies of these files and output them to **/original_copy** and from now on, will only use the copied files for analysis.

The copied files will be hashed and run against the hash of the original files in the database, if there is a discrepancy between the files, Data Hound will let you know - this is the first indication of file tampering. Data Hound will then scan the audio file for any hidden files within it, using magic numbers. 
The hidden file type will be identified and any hidden data will be extracted and placed in the **/extracted** folder. If an audio file has an encrypted file hidden within it, Data Hound will, if prompted complete a brute force attack on the password and then extract the decrypted file and place in the **/extracted** folder.

<p align="center">
<img align="middle" width="600" src="https://github.com/R3D1AM/Blog/blob/main/_resources/DataHound_Flow.png"/>
</p>

I've not had the time to work on it since graduating, but I'm still proud of the little guy - Alas, maybe in the future DataHound will get some more love. We'll see.

