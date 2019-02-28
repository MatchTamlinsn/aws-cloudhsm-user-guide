# Supported PKCS \#11 Mechanisms<a name="pkcs11-mechanisms"></a>

The AWS CloudHSM software library for PKCS \#11 supports the following PKCS \#11 mechanisms\.

**Generate, Create, Import Keys**
+ `CKM_AES_KEY_GEN`
+ `CKM_DES3_KEY_GEN`
+ `CKM_EC_KEY_PAIR_GEN`
+ `CKM_GENERIC_SECRET_KEY_GEN`
+ `CKM_RSA_X9_31_KEY_PAIR_GEN`
**Note**  
This mechanism is functionally identical to the `CKM_RSA_PKCS_KEY_PAIR_GEN` mechanism, but offers stronger guarantees for `p` and `q` generation\. If you need the `CKM_RSA_PKCS_KEY_PAIR_GEN` mechanism, use `CKM_RSA_X9_31_KEY_PAIR_GEN`\.

**Sign/Verify**
+ `CKM_RSA_PKCS`
+ `CKM_RSA_PKCS_PSS`
+ `CKM_SHA256_RSA_PKCS`
+ `CKM_SHA224_RSA_PKCS`
+ `CKM_SHA384_RSA_PKCS`
+ `CKM_SHA512_RSA_PKCS`
+ `CKM_SHA1_RSA_PKCS_PSS`
+ `CKM_SHA256_RSA_PKCS_PSS`
+ `CKM_SHA224_RSA_PKCS_PSS`
+ `CKM_SHA384_RSA_PKCS_PSS`
+ `CKM_SHA512_RSA_PKCS_PSS`
+ `CKM_SHA_1_HMAC`
+ `CKM_SHA224_HMAC`
+ `CKM_SHA256_HMAC`
+ `CKM_SHA384_HMAC`
+ `CKM_SHA512_HMAC`
+ `CKM_ECDSA`
+ `CKM_ECDSA_SHA1`
+ `CKM_ECDSA_SHA224`
+ `CKM_ECDSA_SHA256`
+ `CKM_ECDSA_SHA384`
+ `CKM_ECDSA_SHA512`

**Digest**
+ `CKM_SHA1`
+ `CKM_SHA224`
+ `CKM_SHA256`
+ `CKM_SHA384`
+ `CKM_SHA512`

**Note**  
Data under 16 KB in length are hashed on the HSM, while larger data are hashed locally in software\.

**Encrypt/Decrypt**
+ `CKM_AES_CBC`
+ `CKM_AES_CBC_PAD`
+ `CKM_AES_EBC`
+ `CKM_AES_GCM`
**Note**  
When performing AES\-GCM encryption, the HSM does not accept initialization vector \(IV\) data from the application\. It is required to use an IV that it generates\. The 12\-byte IV provided by the HSM is written into the memory reference pointed to by the pIV element of the CK\_GCM\_PARAMS parameters structure that you supply\. To ensure there is no user confusion, PKCS\#11 SDK in version 1\.1\.1 and later enforce that pIV points to a zeroized buffer when AES\-GCM encryption is initialized\.
+ `CKM_DES3_CBC`
+ `CKM_DES3_CBC_PAD`
+ `CKM_RSA_OAEP_PAD`
+ `CKM_RSA_PKCS`

**Key Derive**
+ `CKM_ECDH1_DERIVE`
**Note**  
This mechanism is implemented to support SSL/TLS Offload cases and is executed only partially within the HSM\. Before using this mechanism, see *Issue: ECDH key derivation is executed only partially within the HSM* in [Known Issues for the PKCS \#11 SDK](KnownIssues.md#ki-pkcs11-sdk)\.

**Key Wrap**
+ `CKM_AES_KEY_WRAP`
+ `CKM_RSA_AES_KEY_WRAP`
+ `CKM_RSA_PKCS_OAEP`

  The following `CK_MECHANISM_TYPE` and `CK_RSA_PKCS_MGF_TYPE` are supported as `CK_RSA_PKCS_OAEP_PARAMS` for `CKM_RSA_PKCS_OAEP`\.
  + `CKM_SHA_1` using `CKG_MGF1_SHA1`
  + `CKM_SHA224` using `CKG_MGF1_SHA224`
  + `CKM_SHA256` using `CKG_MGF1_SHA256`
  + `CKM_SHA384` using `CKM_MGF1_SHA384`
  + `CKM_SHA512` using `CKM_MGF1_SHA512`