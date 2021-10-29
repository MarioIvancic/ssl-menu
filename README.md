# ssl-menu
## Version 1.0.1


This is a simple Perl script for SSL certificate management. 

It is a wrapper around **openssl** tool.

Features:
*    - make simple CA
*    - make CSR and KEY for signing by this or some other CA
*    - make self-signed certificate
*    - sign CSR by this CA
*    - convert KEY, CSR, CRT from various formats to other formats
*    - print KEY, CSR, CRT to console
    
Default encoding is **PEM** for all KEY, CSR, CRT, but **DER** can be used too.
Certificate can also be converted to/from **PKCS#12** encoding.
Files will have extensions key, pub.key, crt, csr, depending on file type.

CA KEY will be 4096 bit RSA key. All other keys will default to 2048 bits 
RSA keys, but can be any of 512, 1024, 2048, 4096 bits for RSA ans DSA
or 256 bits for EC and ED.

**Conversion from CRT to CSR is not fully functional in openssl v1.x.x.**

## Openssl commands

print KEY:
```sh
openssl pkey -text -noout -in $key
```

print CRT
```sh
openssl x509 -text -noout -in $crt
```

print CSR
```sh
openssl req -text -noout -in $csr
```
    
convert KEY from PEM/DER to DER/PEM
```sh
openssl pkey -inform $infmt -in $inkey -outform $outfmt -out $outkey
```

convert CSR from PEM/DER to DER/PEM
```sh
openssl req -inform $infmt -in $incsr -outform $outfmt -out $outcsr
```

convert CSR from PEM to P12
```sh
openssl pkcs12 -export -name $name -inkey $inkey -in $incrt -out $outcrt
```

convert CRT from p12 to PEM
```sh
openssl pkcs12 -nocerts -nodes -in $incrt -out $outkey
openssl pkcs12 -nokeys -clcerts -in $incrt -out $outcrt
```

convert CRT from PEM/DER to DER/PEM
```sh
openssl req -inform $infmt -in $incrt -outform $outfmt -out $outcrt
```

convert CRT to CSR
```sh
openssl x509 -x509toreq -extfile $fn -extensions crt_ext -inform PEM -in $crt -signkey $key -outform PEM -out $csr
```

make new KEY
```sh
openssl pkey -inform $fmt -outform $fmt -in $key -pubout $opts -out $pubkey
```

make new CSR
```sh
openssl req -new -nodes -config $fn -nameopt utf8 -utf8 -outform PEM -out $csr $key
```

make new CA
```sh
openssl req -x509 -config $fn -sha256 -new -newkey rsa:4096 -nodes -nameopt utf8 -utf8 -outform PEM -days 9999 -keyout $ca_dir/$ca_key -out $ca_dir/$ca_cer
```

make new CRT
```sh
openssl req -x509 -config $fn -extensions req_ext -outform PEM -in $csr -key $key -nodes -nameopt utf8 -utf8 -days $days -out $crt
openssl x509 -req -extfile $fn -extensions req_ext -outform PEM -in $csr -CA $ca_dir/$ca_cer -CAkey $ca_dir/$ca_key -CAcreateserial -days $days -out $crt
```
