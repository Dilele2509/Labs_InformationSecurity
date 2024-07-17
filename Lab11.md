# 21110114, Le Thuy Tuong Vy
# Lab 11: Encrypt Large Message

## Task 1: Encrypt and Decrypt Text file
First, I create a plain.txt file with the content shown in the image below:</br>
<img width="726" alt="lab11_1.1" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_1.1.png"><br>

Next, I use various encryption modes such as CBC, ECB, CFB, OFB, and CTR with the following parameters:

<span style="color:yellow">K: 00112233445566778899aabbccddeeff</span></br>
<span style="color:yellow">IV: 010203040506070809aabbccddeeff00</span></br>

Here are some encryption commands that use these modes with plain.txt as the input.</br>
<img width="726" alt="lab11_1.2" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_1.2.png"><br>

As you can see, the input size is 17 bytes. After encrypting with CFB, OFB, and CTR modes, the decrypted file sizes are the same as the input file. However, the decrypted files using CBC and ECB modes are 15 bytes larger than plain.txt.</br>

<img width="726" alt="lab11_1.3" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_1.3.png"><br>

Below are the hex values of the input file and the decrypted files.

<img width="726" alt="lab11_1.4" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_1.4.png"><br>

<span style="color:blue"><b>So, we can see that:</b></span></br>
| Encryption Method | Description |
|-------------------|-------------|
| ECB               | When encrypting a file using ECB, the encrypted file size is padded with an extra 15 bytes compared to the original file. The repeated content in the plaintext is also repeated in the ECB encrypted file, using the same key. |
| CBC               | When encrypting a file using ECB, the encrypted file size is padded with an additional 15 bytes compared to the original file. The repeated content in the plaintext is entirely different in the encrypted file. |
| CFB               | The encrypted file size remains unchanged compared to the original file. The repeated content in the plaintext is entirely different in the encrypted file. |
| OFB               | The size remains unchanged. The first 16 bytes of OFB are the same as CFB, and the content is not repeated. |
| CTR               | The size remains unchanged. The first 16 bytes of CTR are the same as CFB and OFB, and the content is not repeated. |


## Task 2: Encryption Mode – ECB vs. CBC
The Bitmap file consists of three parts: the Bitmap file header, the DIB header, and the bitmap data.</br>
<img width="550" alt="lab11_bitmap" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_bitmap.png"><br>
The first 54 bytes of a bitmap file contain information about the image's header. After encrypting, the header of the original image needs to be replaced with that of the encrypted image to view it properly.</br>

<span style="color:red"><b>1. Electronic Code Book (ECB)</b></span></br>
First, extract the first 54 bytes from the original image file and save them to a file named head_plain.bin. Below is the hex value of the 54-byte header.</br>
<img width="726" alt="lab11_2.1" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_2.1.png"><br>

Next, encrypt "whale.bmp" using ECB mode. Then, retrieve the body part of the cipher, merge it with the original header, and save it in .bmp format.</br>
<img width="726" alt="lab11_2.2" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_2.2.png"><br>

Here is the image “whale.bmp” after encryption using ECB mode:</br>
<img width="726" alt="lab11_2.3" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_2.3.bmp"><br>

<span style="color:red"><b>2. Cipher Block Chaining (CBC)</b></span></br>
Using CBC mode, I followed the same steps as with ECB mode.</br>
<img width="726" alt="lab11_2.4" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_2.4.png"><br>

Here are the results when using CBC mode:
<img width="726" alt="lab11_2.5" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_2.5.bmp"><br>

<span style="color:blue"><b>So, we can see that:</b></span></br>
| Method | Image | Description |
|--------|-------|-------------|
| Original | ![original](https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_whale.bmp)| |
| ECB | ![original](https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_2.3.bmp)| Each block is encrypted with the same key and an independent algorithm, so the encrypted file still retains some of its original structure, making it less secure for data encryption.|
| CBC | ![original](https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_2.5.bmp)| After encryption, the image is completely distorted. This method is safer because it performs XOR before encrypting the blocks.|

## Task 3: Encryption Mode – Corrupted Cipher Text 

<span style="color:red"><b>1. Cipher Block Chaining (CBC)</b></span></br>
First, I create a text.txt file, and here is its hex value.</br>
<img width="726" alt="lab11_3.1" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_3.1.png"><br>
Next, I encrypt the plaintext using CBC mode, following the same steps as before.</br>
<img width="726" alt="lab11_3.2" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_3.2.png"><br>
Then, I use the "dd" command to introduce an error in the 5th byte of the encrypted file. For instance, I change <span style="color:yellow">'e4'</span> to <span style="color:yellow">'fe'</span> in the file <span style="color:yellow">'imvyle-cbc.enc'</span>.</br>
<img width="726" alt="lab11_3.3" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_3.3.png"><br>
Here is the result after decrypting the corrupted file.</br>
<img width="726" alt="lab11_3.4" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_3.4.png"><br>

<span style="color:red"><b>2. Electronic Code Book (ECB)</b></span></br>
Similarly, I follow the same process for CBC mode: encrypt the file, corrupt the 5th byte in the encrypted file, and then decrypt the corrupted file.</br>
<img width="726" alt="lab11_3.5" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_3.5.png"><br>
<img width="726" alt="lab11_3.6" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_3.6.png"><br>
<img width="726" alt="lab11_3.7" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_3.7.png"><br>

<span style="color:red"><b>3. Cipher Feedback (CFB)</b></span></br>

<img width="726" alt="lab11_3.8" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_3.8.png"><br>
<img width="726" alt="lab11_3.9" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_3.9.png"><br>
<img width="726" alt="lab11_3.10" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_3.10.png"><br>

<span style="color:red"><b>4. Output Feedback (OFB)</b></span></br>

<img width="726" alt="lab11_3.11" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_3.11.png"><br>
<img width="726" alt="lab11_3.12" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_3.12.png"><br>
<img width="726" alt="lab11_3.13" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_3.13.png"><br>

<span style="color:blue"><b>So, we can compare 4 different cipher modes:</b></span></br>
| Cipher mode | Image | Description |
|-------------|-------|-------------|
| Original | ![original](https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_comp1.png)| |
| CBC | ![CBC](https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_comp2.png)| The first block containing the corrupted bytes is significantly damaged. The second block is affected by only one byte, specifically at position 5 within that block.|
| ECB | ![ECB](https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_comp3.png)| Even though only the 5th byte is corrupted, after decryption, the entire first block is compromised.|
| CFB | ![CFB](https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_comp4.png)| In the first block, bytes are damaged at the 5th position, but in the second block, the damage is more extensive. The first block is only damaged at position 5 by one byte.|
| OFB | ![OFB](https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_comp5.png)| Only the corrupted bytes are affected => In the best-case scenario, almost all of the text can be recovered.|


<span style="color:blue"><b>Explanation:</b></span></br>
1. **Electronic Codebook (ECB):**
- ECB mode operates by independently encrypting each plaintext block, so a corruption in the 5th byte will only affect that specific block. Recovery is possible for all other unaffected blocks.
- Identical plaintext blocks produce identical ciphertext blocks, potentially exposing the underlying structure of the plaintext.
- It excels in parallel processing, allowing each block to be encrypted independently, which enhances speed for handling large datasets.
- However, it lacks diffusion, meaning small changes in plaintext lead to predictable changes in the ciphertext.</br>
<img width="726" alt="lab11_exp1" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_exp1.png"><br>

2. **Cipher Block Chaining (CBC):**
- CBC mode XORs each plaintext block with the previous ciphertext block before encryption. Therefore, if the 5th byte is corrupted, it affects the entire subsequent block, restricting recovery to blocks preceding the corruption.
- Chaining adds a layer of security as each block's encryption depends on the previous block's ciphertext.
- A single-bit error in one block propagates through the entire chain of subsequent blocks, compounding the impact.
- Unlike ECB, CBC does not support parallelization due to its reliance on the previous ciphertext block.</br>
<img width="726" alt="lab11_exp2" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_exp2.png"><br>

3. **Cipher Feedback (CFB):**
- CFB mode offers error containment: corruption in the 5th byte affects only the current block, preserving the integrity of subsequent blocks up to the point of corruption.
- Each block's encryption depends on the previous ciphertext block, but errors do not propagate beyond the current block boundary.
- It supports bit-level encryption, allowing variable-length plaintext to be processed bit by bit.
- While partially parallelizable, it does not match ECB's level of parallel processing capability.</>
<img width="726" alt="lab11_exp3" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_exp3.png"><br>

4. **Output Feedback (OFB):**
- OFB mode operates without chaining between blocks. Thus, corruption in the 5th byte affects only the current block, allowing recovery up to the point of corruption.
- Similar to CFB, errors within one block do not affect the integrity of other blocks.
- OFB is highly parallelizable since there are no inter-block dependencies.
- It supports bit-level encryption and can efficiently handle variable-length plaintext.</br>
<img width="726" alt="lab11_exp4" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab11_exp4.png"><br>
