# Lithic_challenge

### How I found the challenge 
When I found the job posting, the code actually wasn't hidden yet. There was a line of clearly encoded text underneath the **Crack the code** section of the posting. However I didn't realize that the code was initally incomplete, ending after step 6 but before the "A strong submission..." paragraph. As such, there was no actual script in the decoded text, just the commented description of the challenge. Once I later realized that the encoded text was incomplete (the process of which I'll describe in the next section) I went back to the job posting page to see if I had perhaps copied the string incorrectly. I instead found that it was gone from the spot in the job posting that it had been, at which point I realized what had likely happened; that the encoded text shouldn't have been in that spot on the job posting and was now hidden. Once I realized that, I opened the chrome element inspector and ctrl+f searched for the part of the code I already had, which was shown to me to be hidden in the link on the words "problem-solving" in the **What You'll Bring** section.

### How I decoded the string
I first put the code into chatGPT to ask what it was and how to decode it, where I was told that it was Base64 and given the decoded text and a short python script to decode it locally on my computer. 
```
{
import base64

s = "IyEvdXNyL2Jpbi9lbnYgcHl0aG9uMwoiIiIK..."
print(base64.b64decode(s).decode())
}
```
Through Googling I also found a website to decode Base64 which I used to compare to the decoded text I got from chatGPT for validity. I then tried running this script but recieved an `Incorrect padding` error, which after some Googling I found to be because the encoded text wasn't the right length to be divided into bits to be decoded. I then found a python script to remove any accidental whitespace and adjust the padding. After I ran the encoded text through that I was able to decode it locally and save it so that I could later upload it to this repo. In the next step I decided to run everything in git bash so I could easily push everything to the repo. But when I ran the script given in step 4 of the challenge description, I got an error of `SyntaxError: EOF while scanning triple-quoted string literal` so I realized the decoded text needed to end in triple-quotes, which I added and ran the script again, but nothing happened. I realized that it was setting an enivromental variable and passing my name via the `--candidate` argument, but it didn't seem like that was doing anything. Up until then, I thought the extra steps of adjusting the encoded text and adding the quotes so it could properly be decoded and run were part of the challenge. But this was the point at which I checked the job posting again and found that the encoded text had been hidden and what I had copied was incomplete. After that, I put the full encoded text into the same decoding website I found earlier and saw that it was much more complete so I ran the python script I'd gotten and decoded the full challenge description and script. 

### What this script does
`password` and `_key` are strings of bytes.  
`decrypt_password` pairs the first two bytes of `password` with the first two bytes of `_key` and performs `XOR` on them, getting `0x34` and `0x32` which are returned by `bytes().decode()` as 42.  
`generate_proof` takes the name that was passed when the script is run, along with the decrypted answer, 42, as arguments. It then normalizes the name by removing any spaces from the beginning or end of the name and makes every letter lowercase, or returns an error if no name was passed. It then takes that normalized name, along with 42 and uses those to build a string which is then converted into a hash digest, which is itself converted into a hexidecimal string, the first 12 characters of which are returned as the "proof".  
`main` begins with a check if the environmental variable `DONT PANIC` has a value of `1`. If not, it raises an error instructing one to set it properly and try again. 
