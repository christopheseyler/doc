create the folder if doesn't exist


cd ~/.ssh


ssh-keygen -t rsa -b 4096 -C "example@example.com"


ssh-keygen -t rsa -b 4096 -C "christophe.seyler@gmail.com"

choose your file name


Ignore passphrase (by press Enter key without input any characters) if you want to create a non-passphrase ssh key


eval `ssh-agent -s` 

ssh-add MyFilename


to list the keys
ssh-add -l


