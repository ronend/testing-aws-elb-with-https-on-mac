# valid-https-mac-for-aws-elb
Based on https://stackoverflow.com/questions/7580508/getting-chrome-to-accept-self-signed-localhost-certificate/43666288#43666288
light weight tutorial for having secure connection from you mac to aws elb

1. Create root cert authority:
```bash
./create_root_cert_and_key.sh
Generating RSA private key, 2048 bit long modulus
..........................+++
....................................................................+++
e is 65537 (0x10001)
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) []:US
State or Province Name (full name) []:NY
Locality Name (eg, city) []:New York
Organization Name (eg, company) []:SampleCompanyName
Organizational Unit Name (eg, section) []:SampleOrgUnitName
Common Name (eg, fully qualified host name) []:[region_name].elb.amazonaws.com                                              
Email Address []:john@company.com
```
1. Create the website certificate:
```bash
./create_certificate_for_domain.sh [elb-dns] [elb-dns] # e.g. [elb-name].us-east-1.elb.amazonaws.com
```
1. Add root CA to the key chain:
```bash
sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain rootCA.pem
```
1. Import the certificate to ACM:
```bash
# copy to the Certificate body box
cat [elb-name].us-east-1.elb.amazonaws.com.crt
# copy to the Certificate private key box
cat device.key
# copy to the Certificate chain
cat rootCA.pem
```
