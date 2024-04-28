---
title: Transfer data via gpg
date: 2023-04-03 19:14:18
tags:
    - gpg 加密
    - Linux
declare: true
---
Here are the steps to encrypt data with GPG on CentOS 7 and transfer it with friends:
1. Install GPG
First, GPG needs to be installed on CentOS 7. It can be installed with the following command:
`sudo yum install gnupg2`
<!--more-->
2. Generate a key pair
To encrypt and decrypt using GPG, a key pair needs to be generated. A key pair can be generated with the following command:
`gpg --full-generate-key`

Enter your name, email address, and password when prompted. You can choose the default options, or make custom configurations according to your needs. Generating the key pair may take some time, depending on the speed of your computer and the encryption algorithm selected.

3. Export the public key
After generating the key pair, you need to export your public key and share it with your friends so that they can use your public key to decrypt the encrypted data you send. You can export your own public key with the following command:
`gpg --armor --output mypubkey.gpg --export myemail@example.com`

Where **myemail@example.com** is the email address you entered when generating the key pair, you can replace it with the actual value. The **--armor** parameter converts the output to ASCII for sharing in email or text messages.

Send the **mypubkey.gpg** file to your friends.

4. Key import
Your friend needs to import your public key into their GPG keyring to be able to encrypt data with your public key and send it to you. The public key can be imported with the following command:
`gpg --import mypubkey.gpg`

where **mypubkey.gpg** is the name and path of your exported public key file.

5. Encrypted data
Now you can encrypt your data with GPG and send it to your friends. Files can be encrypted with the following command:
`gpg --output myfile.gpg --encrypt --recipient myemail@example.com myfile.txt`

where myfile.txt is the name and path of the file to encrypt, and myemail@example.com is your friend's email address, which can be replaced with actual values. The --output parameter specifies the output filename of the encrypted file, the --encrypt parameter specifies the file to be encrypted, and the --recipient parameter specifies the recipient's email address.

Send the encrypted myfile.gpg file to your friend.

6. Decrypt data
Your friend can use GPG to decrypt received encrypted data. The file can be decrypted with the following command:
`gpg --output myfile.txt --decrypt myfile.gpg`

where myfile.gpg is the name and path of the received encrypted file and myfile.txt is the output filename of the decrypted file.

That concludes the steps to use GPG to encrypt data and transfer with friends on CentOS 7.