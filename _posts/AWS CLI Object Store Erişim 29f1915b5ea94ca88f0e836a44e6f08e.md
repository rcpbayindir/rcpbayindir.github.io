# AWS CLI Object Store Erişim

Kaynak: [https://docs.datafabric.hpe.com/70/AdvancedInstallation/Enabling_object_store.html](https://docs.ezmeral.hpe.com/datafabric-customer-managed/72/AdvancedInstallation/Enabling_object_store.html)

Ön hazırlık olarak ortamda java 11 yüklenmelidir.

1. İlk olarak aşağıdaki linkten aws cli kurulumu yapılır:

[https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

1. İndirme ve kurulum tamamlandıktan sonra cmd üzerinden konfigürasyon işlemleri yapılır. Access key ve secret key kullanıcı adı ile gönderilen json dosyası içerisinden alınmalıdır. Region: None ve output format: json olarak girilmelidir.

```bash
aws configure
```

![Untitled](AWS%20CLI%20Object%20Store%20Eris%CC%A7im%2029f1915b5ea94ca88f0e836a44e6f08e/Untitled.png)

1. Sonrasında enviroment path olarak JAVA_HOME tanımı bilgisayarınızda kontrol edilmeli. Yok ise tanımlanmalıdır. (Program Files içerisinde ise boşluk hataya sebep olabilir bu nedenle resimdeki gibi boşluksuz şekilde tanımlanmalıdır. )

![Untitled](AWS%20CLI%20Object%20Store%20Eris%CC%A7im%2029f1915b5ea94ca88f0e836a44e6f08e/Untitled%201.png)

1. Aşağıdaki komut cmd üzerinden çalıştırılmalıdır. 

-file parametresinden sonrası (<pem_file_path>) size atılan pem dosyasının path’i olacak şekilde değiştirilmelidir.

 

```bash
%JAVA_HOME%\bin\keytool.exe -noprompt -importcert -file <pem_file_path> -alias maprca -keystore %JAVA_HOME%\lib\security\cacerts 
-storepass changeit
```

1. Gönderilen “ssl_truststore.pem” dosyası AWS_CA_BUNDLE değişkeni olarak ortam değişkenlerine tanımlanması.

![Untitled](AWS%20CLI%20Object%20Store%20Eris%CC%A7im%2029f1915b5ea94ca88f0e836a44e6f08e/Untitled%202.png)

1. Aşağıdaki komutları kullanarak bucket ve object listleyebilir, obje yükleyebilirisiniz.

```bash

# yetkili olunan bucket isimlerini listeler
aws s3api list-buckets --endpoint-url https://toggdfnode8.togg.com.tr:9000

# eğer yetki tanımlandıysa bucket altındaki objeleri listeler
aws s3api list-objects-v2 --bucket plm-bucket --endpoint-url https://toggdfnode8.togg.com.tr:9000

# eğer yetki tanımlandıysa bucket içerisine dosya yükler 
aws s3api put-object --bucket plm-bucket --key <filepath/filename> --body <filename> --endpoint-url https://toggdfnode8.togg.com.tr:9000

aws s3api delete-object  --bucket vehicle-engineering-bucket --key  vehicle-durability/CSVreport.csv --endpoint-url https://toggdfnode8.togg.com.tr:9000

```

```jsx

alternative --config java
/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.372.b07-1.el8_7.x86_64/jre/bin/java

sudo vi ~/.bashrc

export AWS_USE_FIPS_ENDPOINT=https://toggdfnode8.togg.com.tr:9000
export AWS_CA_BUNDLE=/home/dataadmin/delmia_BOP_BOR_MBOM/ssl_files/chain-ca.pem
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.372.b07-1.el8_7.x86_64/jre/bin/java
export PATH="$JAVA_HOME/bin:$PATH"

source ~/.bashrc

echo $JAVA_HOME

cd /home/dataadmin/delmia_BOP_BOR_MBOM

sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update

${JAVA_HOME}/jre/bin/keytool -noprompt -importcert -file ssl_files/chain-ca.pem -alias maprca -keystore ${JAVA_HOME}/jre/lib/security/cacerts -storepass changeit

${JAVA_HOME}/jre/bin/keytool -noprompt -importcert -file ssl_files/ssl_keystore.pem -alias maprca -keystore ${JAVA_HOME}/jre/lib/security/cacerts -storepass changeit

$JAVA_HOME/jre/bin/keytool -noprompt -importcert -file ssl_files/ssl_keystore.pem -alias maprca -keystore $JAVA_HOME/jre/lib/security/cacerts -storepass changeit

-noprompt -importcert -file /home/dataadmin/delmia_BOP_BOR_MBOM/ssl_files/ssl_keystore.pem -alias maprca -keystore $JAVA_HOME/jre/lib/security/cacerts -storepass changeit

${JAVA_HOME}/jre/bin/keytool -delete -alias maprca -keystore ${JAVA_HOME}/jre/lib/security/cacerts
```

```python
çalışan

/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.372.b07-1.el8_7.x86_64/jre/bin/keytool -delete -alias maprca -keystore /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.372.b07-1.el8_7.x86_64/jre/lib/security/cacerts 

keytool -noprompt -importcert -file ssl_files/chain-ca.pem -alias maprca -keystore /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.372.b07-1.el8_7.x86_64/jre/lib/security/cacerts -storetype PKCS12 -storepass changeit

/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.372.b07-1.el8_7.x86_64/jre/bin/keytool -list -keystore /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.372.b07-1.el8_7.x86_64/jre/lib/security/cacerts

keytool -v -list -keystore /path/to/keystore

/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.372.b07-1.el8_7.x86_64/jre/bin/keytool -storepasswd -v -new Togg2023 -keystore /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.372.b07-1.el8_7.x86_64/jre/lib/security/cacerts

/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.372.b07-1.el8_7.x86_64/jre/bin/keytool -importkeystore -srckeystore  /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.372.b07-1.el8_7.x86_64/jre/lib/security/cacerts -destkeystore /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.372.b07-1.el8_7.x86_64/jre/lib/security/cacertsv2

keytool -noprompt -importcert -file ssl_files/chain-ca.pem -alias maprca2 -keystore /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.372.b07-1.el8_7.x86_64/jre/lib/security/cacerts -storetype PKCS12 -storepass changeit
```

```python
%JAVA_HOME%/bin/keytool.exe -noprompt -importcert -file D:\ezmeral\ssl_truststore.pem -alias mosscert -keystore %JAVA_HOME%/lib/security/cacerts -storepass changeit

aws s3api list-buckets --ca-bundle /home/dataadmin/delmia_BOP_BOR_MBOM/ssl_files/ssl_keystore.pem  --endpoint-url https://toggdfnode8.togg.com.tr:9000

aws s3api list-objects-v2 --bucket uep-data --ca-bundle /opt/mapr/conf/ssl_truststore.pem --endpoint-url https://togghpedfapollo05.togg.com.tr:9000 

AWS_USE_FIPS_ENDPOINT=https://toggdfnode8.togg.com.tr:9000
AWS_CA_BUNDLE https://toggdfnode8.togg.com.tr:9000
```