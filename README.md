# auto-backup-mikrotik-ke-email

Tutor Seting Emailnya kesini
https://youtu.be/HDLnYcB9ytY?t=201


```
/system clock
set time-zone-name=Asia/Jakarta

/system ntp client
set enabled=yes server-dns-names=id.pool.ntp.org

/system scheduler
add interval=1d name=auto_backup on-event="/system script run backup" policy=\
    ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon \
    start-date=may/29/2023 start-time=01:00:00

/system script
add dont-require-permissions=no name=backup owner=admin policy=\
    ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon source="#\
    ### Modify these values to match your requirements ####\r\
    \n\r\
    \n#Your email address to receive the backups\r\
    \n:local toemail \"emailmu@gmail.com\"\r\
    \n\r\
    \n#The From address (you can use your own address if you want)\r\
    \n:local fromemail \"MikrotikBackup\"\r\
    \n\r\
    \n#A mail server your machines can send through\r\
    \n:local emailserver \"smtp.gmail.com\"\r\
    \n\r\
    \n############## Don\92t edit below this line ##############\r\
    \n\r\
    \n:local textfilename\r\
    \n:local backupfilename\r\
    \n:local allfiles\r\
    \n:local filedate \r\
    \n:local date [/system clock get date];\r\
    \n:local time [/system clock get time];\r\
    \n:local sysname [/system identity get name];\r\
    \n\r\
    \n:local months (\"jan\",\"feb\",\"mar\",\"apr\",\"may\",\"jun\",\"jul\",\
    \"aug\",\"sep\",\"oct\",\"nov\",\"dec\");\r\
    \n:local varMonth [:pick \$date 0 3];\r\
    \n:set varMonth ([ :find \$months \$varMonth -1 ] + 1);\r\
    \n\r\
    \n:if (\$varMonth < 10) do={ :set varMonth (\"0\" . \$varMonth); }\r\
    \n\r\
    \n:local varDay [:pick \$date 4 6];\r\
    \n:local varYear [:pick \$date 7 11];\r\
    \n:set filedate (\$varYear.\"-\".\$varMonth.\"-\".\$varDay);\r\
    \n\r\
    \n\r\
    \n:set textfilename (\$\"filedate\" . \"-\" . \"CCR1072_Bandung\" . \".rsc\
    \")\r\
    \n:set backupfilename (\$\"filedate\" . \"-\" . \"CCR1072_Bandung\" . \".ba\
    ckup\")\r\
    \n:set allfiles (\$\"textfilename\" . \",\" . \$\"backupfilename\")\r\
    \n:execute [/export file=\$\"textfilename\"]\r\
    \n:execute [/system backup save name=\$\"backupfilename\"]\r\
    \n#Allow time for export to complete\r\
    \n:delay 2s\r\
    \n\r\
    \n#email copies\r\
    \n:log info \"Emailing backups\"\r\
    \n/tool e-mail send to=\$\"toemail\" from=\$\"fromemail\" server=[:resolve\
    \_\$emailserver] port=587 subject=\"[Config Backup] CCR1072_Bandung\" file=\$\
    \"allfiles\"\r\
    \n#Allow time to send\r\
    \n:delay 10s\r\
    \n\r\
    \n#delete copies\r\
    \n/file remove \$textfilename\r\
    \n/file remove \$backupfilename"
/tool e-mail
set address=smtp.gmail.com from=<CCR1072_Bandung> password=nsthstnrsntsn \
    port=587 start-tls=yes user=emailmu@gmail.com
```
