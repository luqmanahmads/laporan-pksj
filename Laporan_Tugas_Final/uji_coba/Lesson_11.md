# Lesson 11 : SQL Injection Union Exploit #4 (Create PHP Upload Script)

## Download c99.php
1. Open a console terminal
2. Get Rar File
- Buka terminal, jalankan perintah berikut :
```
mkdir -p /root/backdoor
cd /root/backdoor/
wget http://www.computersecuritystudent.com/SECURITY_TOOLS/MUTILLIDAE/MUTILLIDAE_2511/lesson11/stuff.rar
ls -lrt
```
3. Extract Rar File
   - Jalankan perintah berikut pada terminal :
```
unrar x stuff.rar
cat part1.txt part2.txt part3.txt > c99.php
cp c99.php c99.php.bkp
ls -lrt
```
4. Configure and Prepare c99.php