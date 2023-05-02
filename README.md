# What is TLS 1.3 and its requirements:

Transport Layer Security (TLS) is an essential security protocol used to secure communication over the internet. It is used to establish a secure encrypted connection between a client and a server, ensuring that sensitive data remains confidential and secure. TLS version 1.3 is the latest and most secure version of the protocol. In this blog post, we'll explore why TLS 1.3 is needed and the benefits it offers and how exactly it works.

Firstly, it is important to understand that the internet was designed to be an open system, where data can be freely exchanged between different computers and networks. However, this openness also means that anyone can intercept and read the data being transmitted, including sensitive information such as passwords, credit card details, and personal information. This is where TLS comes in, as it provides a secure way to transmit this data over the internet.

TLS 1.3 offers several advantages over previous versions of the protocol. Firstly, it improves security by removing outdated and vulnerable features, such as support for weak encryption algorithms and insecure key exchange mechanisms. This means that TLS 1.3 is less vulnerable to attacks, such as the notorious POODLE attack that affected TLS 1.2.

TLS 1.3 also offers faster connection times, as it reduces the number of round-trips required during the handshake process. This is achieved by using a feature called "0-RTT" (zero round-trip time), which allows clients to resume previous sessions without requiring a full handshake. This results in faster and more efficient connections, which is especially important for time-sensitive applications such as online gaming and financial trading.

Another significant improvement offered by TLS 1.3 is enhanced privacy. The protocol encrypts more of the handshake process, including the negotiation of the cipher suite and the exchange of session keys. This means that even if an attacker manages to intercept the handshake process, they will not be able to decipher the exchanged information.

Furthermore, TLS 1.3 offers improved support for forward secrecy, a feature that ensures that even if a private key is compromised in the future, past communications will remain secure. This is achieved by generating unique session keys for each session, which are discarded after the session is completed.

In conclusion, TLS 1.3 is essential for securing communication over the internet. It provides improved security, faster connection times, enhanced privacy, and support for forward secrecy. As the internet continues to grow and evolve, it is crucial that we continue to use and improve protocols like TLS to ensure that our online communications remain confidential and secure.

## Steps for TLS 1.3 handshake:

When you visit a website you browser performs the following steps before communicating with the server: 
1. Client: Client Hello
2. Server: Server Hello
3. Server: Server certificate exchange
4. Server: Certificate verify
5. Server: Server finished
6. Client: Client finished


## Client Hello

The first step in the TLS 1.3 handshake process is for your browser to send a message called the Client Hello to the website server. This message contains information about the encryption algorithms that your browser supports and some other details needed for the handshake. The Client also generates its key (let’s call it a) and shares G^a % N to the server. 

The Client Hello message is like a letter that your browser sends to the website server to introduce itself and request a secure connection. 







## Server Hello:

The browser had sent a request to the server asking to establish a secure connection. The server then selects an encryption method from a list of options provided by your browser.

Once the encryption method is selected, the server generates its own private key called "b" and a random number called g^b. The server then sends this information back to your browser so that both the server and your browser can generate the symmetric key used for encryption.

This symmetric key is used to encrypt and decrypt data as it is transmitted between your browser and the server. It ensures that your data remains private and secure while it travels over the internet.





Now from this point as the client and server both have the shared symmetric key they can communicate encrypted with each other. 
So, is the handshake completed?
The answer is no.
## Server Certificate Exchange:


The identity verification of the server is still missing. How can we verify that the data we are receiving is sent by the server?

The digital certificate contains information about the server's identity, such as its domain name, public key, and other details, and is signed by a trusted Certificate Authority (CA).

To verify the authenticity of the server's digital certificate, the client can use the server's public key to decrypt the digital signature on the certificate and validate that it was signed by a trusted CA. If the validation succeeds, the client can be confident that the server is who it claims to be.

So, as now we already have the symmetric keys on both sides, the certificate chain is encrypted by the symmetric key and sent to the client.
## Certificate Verify:

Everything sounds cool till now, we have the keys generated and also verified the certificate chain. Still there’s a problem here, what if someone in between has modified the data in between and is listening to our conversations (yes, that’s the man in the middle attack).

We need to check that all of the data received to the server is the same as sent by the client and securely send this data such that if this message is modified then it won’t make sense.

How should the server do this such that no one another can do the same? 

The answer is the server takes all the data from the previous communication and calculates the hash of the messages and encrypts the hash using the server’s private key and sends it to the client.

The client on the other hand also has all the previous messages from the handshake from which it calculates the hash (must be the same as what the server calculated). 
The client also has the server’s public key (received in certificate chain) using which the client decrypts the encrypted message and verifies the similarity between the hash.
If both the hashes are the same it means the data has been changed in between and verifies integrity of the communication.

This step ensures that the symmetric key is shared only between the client and the server and no one else can see our messages.
## Server Finished:


Now the majority portion of the communication is completed and we need to ensure all the data exchanged during the handshake is the same with both the server and client. 
So, to ensure this the server calculates a hash of all previous handshake messages and sends it to the client. The client verifies the hash value sent by the server.

## Client Finished:
The client also calculates the hash of previous handshake messages up to this point and sends it back to the server. The server on the other hand verifies the hash value sent by the client.

End of handshake, now the secure communication channel is established.The client also calculates the hash of previous handshake messages up to this point and sends it back to the server. The server on the other hand verifies the hash value sent by the client.

