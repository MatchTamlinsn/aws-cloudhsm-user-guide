# AWS CloudHSM use cases<a name="use-cases"></a>

A hardware security module \(HSM\) in AWS CloudHSM can help you accomplish a variety of goals\.

**Topics**
+ [Offload the SSL/TLS processing for web servers](#crypto-offload)
+ [Protect the private keys for an issuing certificate authority \(CA\)](#certificate-authority)
+ [Enable transparent data encryption \(TDE\) for Oracle databases](#transparent-data-encryption)

## Offload the SSL/TLS processing for web servers<a name="crypto-offload"></a>

Web servers and their clients \(web browsers\) can use Secure Sockets Layer \(SSL\) or Transport Layer Security \(TLS\)\. These protocols confirm the identity of the web server and establish a secure connection to send and receive webpages or other data over the internet\. This is commonly known as HTTPS\. The web server uses a public–private key pair and an SSL/TLS public key certificate to establish an HTTPS session with each client\. This process involves a lot of computation for the web server, but you can offload some of this to the HSMs in your AWS CloudHSM cluster\. This is sometimes known as SSL acceleration\. Offloading reduces the computational burden on your web server and provides extra security by storing the server's private key in the HSMs\.

For information about setting up SSL/TLS offload with AWS CloudHSM, see [SSL/TLS offload](ssl-offload.md)\.

## Protect the private keys for an issuing certificate authority \(CA\)<a name="certificate-authority"></a>

In a public key infrastructure \(PKI\), a certificate authority \(CA\) is a trusted entity that issues digital certificates\. These digital certificates bind a public key to an identity \(a person or organization\) by means of public key cryptography and digital signatures\. To operate a CA, you must maintain trust by protecting the private key that signs the certificates issued by your CA\. You can store the private key in the HSM in your AWS CloudHSM cluster, and use the HSM to perform the cryptographic signing operations\.

## Enable transparent data encryption \(TDE\) for Oracle databases<a name="transparent-data-encryption"></a>

Some versions of Oracle's database software offer a feature called Transparent Data Encryption \(TDE\)\. With TDE, the database software encrypts data before storing it on disk\. The data in the database's table columns or tablespaces is encrypted with a table key or tablespace key\. These keys are encrypted with the TDE master encryption key\. You can store the TDE master encryption key in the HSMs in your AWS CloudHSM cluster, which provides additional security\.

For information about setting up Oracle TDE with AWS CloudHSM, see [Oracle database encryption](oracle-tde.md)\.