# ssl-menu
This is a simple Perl script for SSL certificate management. 
It is a wrapper around openssl tool.
Features:
    - make simple CA
    - make CSR and KEY for signing by this or some other CA
    - make self-signed certificate
    - sign CSR by this CA
    - convert KEY, CSR, CRT from various formats to other formats
    - print KEY, CSR, CRT to console
    
Default encoding is PEM for all KEY, CSR, CRT, but DER can be used too.
Certificate can also be converted to/from PKCS#12 encoding.
Files will have extensions key, pub.key, crt, csr, depending on file type.

CA KEY will be 4096 bit RSA key. All other keys will default to 2048 bits 
RSA keys, but can be any of 512, 1024, 2048, 4096 bits for RSA ans DSA
or 256 bits for EC and ED.

Conversion from CRT to CSR is not fully functional in openssl v1.x.x.
