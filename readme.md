So i needed a SSL certificate for my localhost and couldn't find a solution generate a certificate that browser will trust. After a few hours i found out how to generate it correctly
Follow this steps:
1) openssl genrsa -out rootCA.key 2048
2) openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.pem
3) create a file named v3.ext and fill it with:
   authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = %%DOMAIN%%
DNS.2 = *.%%DOMAIN%%
4)cat v3.ext | sed s/%%DOMAIN%%/$COMMON_NAME/g > /tmp/v3.ext
openssl x509 -req -in device.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out device.crt -days $NUM_OF_DAYS -sha256 -extfile /tmp/v3.ext
4)run the sh script and get a file named *yourdomenname*.crt and device.key
5) add them to your localhost settings file by typing next lines (nginx example)

   ![8mv4rkzcwxfnlr6uxkilnenonhs](https://user-images.githubusercontent.com/87041079/200117883-5a7798ad-e9ad-4336-9d50-ca4f1d723b55.png)

6) trust the certificate in system settings![qdn68hbbkgsi7dyzy7hzra8mvv4](https://user-images.githubusercontent.com/87041079/200117875-0e4bac55-c297-4df1-9966-0405dd10baf8.png)
Congratulations, now you have a fully working localhost with ssl certificate
If you have any questions you can dm me in my tg @cvltyxd
