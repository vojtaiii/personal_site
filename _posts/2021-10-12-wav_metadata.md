---
title: "Create a WAV audio file metadata bytewise"
date: 2021-10-12T21:00:00-04:00
categories:
  - tools
tags:
  - wav
  - metadata
  - kotlin
  - java
---

An audio file can support many various metada, such as the artist, album, copyright, year, genre. These are further used by audio streaming softwares to display the info to the user. 

Suppose we have the following scenario. In out software we just created, or have been given, a audio file and want to attach some specific information to it. How to achieve it programatically without using third party softwares or some extensive libraries? In this tutorial we`ll use **Kotlin** as the programming language but the principle should be easy to adapt to any objective language of your interest. 

**Note:** It is expected that you already have the audio file, i.e. containing the mandatory RIFF header and the PCM data. Tutorials on how to create the audio file can be found for example 
[here](wavaudiourl1) or [here](wavaudiourl2).

### The parameters

Suppose we have these three parameters

```kotlin
val artist = "Slayer"
val album = "Abbey road"
val genre = "funky jazz"
```

and want to store them in the corresponding metadata fields artist, album and genre.

### The RIFF file INFO chunk specification

A wav audio file falls into the RIFF files group. RIFF documentation specifies a part of the file, marked as INFO, where the metadata can be stored. A good summary of the so-called INFO chunk can be found on [recordingsblogs](infochunk). 

### Prepare the values 

According to the specification

![alt text][riffheaderpic]

we firstly have to write the chunk ID - LIST and its overall size. The INFO chunk, containing the metadata have the following structure

| Info ID (4 byte ASCII text) for information 1 | 
| Size of the information 1 (4 byte Int) | 
| Information 1 |
| Info ID (4 byte ASCII text) for information 2 | 
| Size of the information 2 (4 byte Int) | 
| Information 2 | 
| ... |

The info IDs can be found in the [specification](infochunk) and the informations are our parameters. So last thing we had to prepare is the size of them. 

In order to write the parameters bytewise they need to be converted into a character array.

```kotlin
// split the parameters into chars
val artistChar = artist.toCharArray()
val albumChar = album.toCharArray()
val genreChar = gendre.toCharArray()
```

and the sizes of the parameters are then

```kotlin
// parameter sizes
val artistSize = artistChar.size
val albumSize = albumChar.size
val genreSize = gendreChar.size
```

Lets now prepare the values to be written to the LIST and INFO chunks in an `infoBytes` array.

```kotlin
val infoBytes: ByteArray = ByteBuffer
            .allocate(16)
            .order(ByteOrder.LITTLE_ENDIAN)
            .putInt((3 * (4+4)) + artistSize + albumSize + genreSize) // size of the whole INFO chunk minus 8
            .putInt(artistSize) // size of the 1st parameter 
            .putInt(albumSize) // size of the 2nd parameter
            .putInt(genreSize) //  size of the 3rd parameter
            .array()
```

### Identify the Info IDs

Looking into the specification, we find that artist corresponds to *IART* field, album to *IPRD* and genre to *IGNR*.

Now we have everything setup to write the data to the file.

### Write the data to the WAV file

First, lets access the specified file `wavFile` and move the pointer to its very end.

```kotlin
val accessWave = RandomAccessFile(wavFile, "rw") // specify the file
accessWave.seek(wavFile.length().toLong()) // move the pointer to the end
```

And now write all the information we have into the file following the specification displayed above. For illustrative reasons it is shown here completely bitewise.

```kotlin
accessWave.write(
                // INFO chunk
                byteArrayOf(
                    'L'.code.toByte(), // add LIST chunk at the end of the file
                    'I'.code.toByte(),
                    'S'.code.toByte(),
                    'T'.code.toByte(),
                    infoBytes[0], // size of the whole chunk minus 8 (Int)
                    infoBytes[1],
                    infoBytes[2],
                    infoBytes[3],
                    'I'.code.toByte(), // specifies the chunk as INFO
                    'N'.code.toByte(),
                    'F'.code.toByte(),
                    'O'.code.toByte(),
                    'I'.code.toByte(), // specifies IART
                    'A'.code.toByte(),
                    'R'.code.toByte(),
                    'T'.code.toByte(),
                    infoLittleBytes[4], // the size of IART field value
                    infoLittleBytes[5],
                    infoLittleBytes[6],
                    infoLittleBytes[7],
                    artistChar[0].code.toByte(), // the first parameter, saved in IART
                    artistChar[1].code.toByte(),
                    artistChar[2].code.toByte(),
                    artistChar[3].code.toByte(),
                    artistChar[4].code.toByte(),
                    artistChar[5].code.toByte(),
                    'I'.code.toByte(), // specifies IPRD
                    'P'.code.toByte(),
                    'R'.code.toByte(),
                    'D'.code.toByte(),
                    infoLittleBytes[8], // the size of IPRD field value (Int)
                    infoLittleBytes[9],
                    infoLittleBytes[10],
                    infoLittleBytes[11],
                    albumChar[0].code.toByte(), // the second parameter, saved in IPRD
                    albumChar[1].code.toByte(),
                    albumChar[2].code.toByte(),
                    albumChar[3].code.toByte(),
                    albumChar[4].code.toByte(),
                    albumChar[5].code.toByte(),
                    albumChar[6].code.toByte(),
                    albumChar[7].code.toByte(),
                    albumChar[8].code.toByte(),
                    albumChar[9].code.toByte(),
                    'I'.code.toByte(), // specifies IGNR
                    'G'.code.toByte(),
                    'N'.code.toByte(),
                    'R'.code.toByte(),
                    infoLittleBytes[12], // the size of IGNR field value (Int)
                    infoLittleBytes[13],
                    infoLittleBytes[14],
                    infoLittleBytes[15],
                    genreSize[0].code.toByte(), // the third parameter, saved in IGNR
                    genreSize[1].code.toByte(),
                    genreSize[2].code.toByte(),
                    genreSize[3].code.toByte(),
                    genreSize[4].code.toByte(),
                    genreSize[5].code.toByte(),
                    genreSize[6].code.toByte(),
                    genreSize[7].code.toByte(),
                    genreSize[8].code.toByte(),
                    genreSize[9].code.toByte()
                )
            )

accessWave.close() // close the stream
```

And thats it! If you observe the audio in a software like Audacity, you`ll see the parameters there, in the metadata field.

![alt text][audacitypic]


[wavaudiourl1]: https://stackoverflow.com/questions/22695723/create-valid-wav-file-header-for-streams-in-memory
[wavaudiourl2]: https://stackoverflow.com/questions/9179536/writing-pcm-recorded-data-into-a-wav-file-java-android
[infochunk]: https://www.recordingblogs.com/wiki/list-chunk-of-a-wave-file
[riffheaderpic]: https://github.com/vojtaiii/personal_site/blob/gh-pages/assets/images/riff_metadata/riff_header.JPG?raw=true
[audacitypic]: https://github.com/vojtaiii/personal_site/blob/gh-pages/assets/images/riff_metadata/audacity.JPG?raw=true


