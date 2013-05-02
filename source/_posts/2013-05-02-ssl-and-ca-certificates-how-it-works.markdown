---
layout: post
title: "SSL and CA Certificates: How it works"
date: 2013-05-02 13:35
comments: true
categories: 
---


The Secure Socket Layer protocol was created by Netscape to ensure secure transactions between web servers and browsers. The protocol uses a third party, a Certificate Authority (CA), to identify one end or both end of the transactions. This is in short how it works.

1. A browser requests a secure page (usually https://).

2. The web server sends its public key with its certificate.

3. The browser checks that the certificate was issued by a trusted party (usually a trusted root CA), that the certificate is still valid and that the certificate is related to the site contacted.

4. The browser then uses the public key, to encrypt a random symmetric encryption key and sends it to the server with the encrypted URL required as well as other encrypted http data.

5. The web server decrypts the symmetric encryption key using its private key and uses the symmetric key to decrypt the URL and http data.

6. The web server sends back the requested html document and http data encrypted with the symmetric key.

7. The browser decrypts the http data and html document using the symmetric key and displays the information.



Your server has a certificate, consisting of a private and a public key. The server never gives out the private key of course, but everyone may get the public key. The public key is embedded within a certificate container format. This format contains the IP address or domain name of the server, the owner of this IP address/domain name, an e-mail address of the owner, etc.

The whole thing is signed by a trusted authority. The trusted authority, aka certificate authority (CA) also has a private/public key pair. You give them your certificate, they verify that the information in the container are correct and sign the it by their private key, only they have access to.

The public key of the CA is installed on the user system by default, most well known CAs are included already in the default installation of your favorite OS or browser.

When now a user connects to your server, your server uses the private key to sign some data, packs that signed data together with its public key and sends everything to the client.

What can the client now do? First of all, it can use the public key it just got sent to verify the signed data. Since only the owner of the private key is able to sign the data correctly in such a way that the public key can verify the signature, it knows that whoever signed this piece of data, this person is owning the private key to the received public key. So far so well. But what stops a hacker to intercept the packet, replace the signed data with data he signed using a different certificate and also replace the public key with his public key? Nothing.

That's why after the signed data has been verified (or before it is verified) the client verifies that the received public key has a valid CA signature. Using the already installed public CA key, it verifies that the received public key has been signed by a known CA. Otherwise it is rejected (as a hacker may have replaced it on the way).

Last but not least, it checks the information within the public key container. Does the IP address or domain name really match the IP address or domain name of the server the client is currently talking to? If not, something is fishy!

People may ask: What stops a hacker from just creating his own keypair and just putting your domain name or IP address into the cert? Easy: If he does that, no CA will sign his key. To get a CA signature, you must prove that you are really the owner of this IP address or domain name. The hacker is not, he cannot prove that, he won't get a signature. So this won't work.

Okay, how about the hacker registers his own domain, create a certificate for that, and have that signed by a CA? This works, he will get it CA signed, it's his domain, no problem. However he cannot use this when hacking your connection. If he uses this certificate, the browser will immediately see that the signed public key is for domain example.net, but it is currently talking to example.com, not the same domain, thus something is fishy again.

More readings:

- [SSL Certificates: HOWTO](http://www.tldp.org/HOWTO/SSL-Certificates-HOWTO/x64.html)
