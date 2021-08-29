# Post Office 

Profile для Volatility: Win10x64_15063 

(2) Просматриваем список запущенных процессов:
`vol.py -f task.mem --profile=Win10x64_15063 pslist`

Находим процесс Outlook (PID: 4500)

(3) Производим дамп памяти процесса Outlook: 
`vol.py -f task.mem --profile=Win10x64_15063 memdump -p 4500 -D .`

(4) Анализируем дамп памяти процесса (например: находим вхождения VBA-макросов). 

(5) Вспоминаем или ищем в Интернете информацию о VBA-макросах и Outlook.

(6) `%SYSDRIVE%:Users\<Username>\AppData\Roaming\Microsoft\Outlook\<Name>.OTM` (в данном случае название скрипта: VBAProject.OTM)

(7) `vol.py -f task.mem --profile=Win10x64_15063 filescan > filescan.txt`

(8) `vol.py -f task.mem --profile=Win10x64_15063 dumpfiles -r OTM$ -D .`

(9) `olevba file.4500.0xffffae07b9686900.dat`

(10) Реверс VBA-скрипта (Base64).

(10.5) Декод флага из https://pastebin.com/raw/ULPAa2tW и строки из скрипта 

(11) **Флаг**: `YauzaCTF{whataboutoutlook}`

# Unexpected Invasion 1

В Event журнале Microsoft-Windows-Bits-Client%4Operational.
**Флаг:** YauzaCTF{AdFind.exe}

# Unexpected Invasion 2


MFT + Syscache.

**Флаг:** YauzaCTF{script_c_2_4ca5ef55431c441f51e1229031288f54807c2f11}

# Unexpected Invasion 3

Dropmefiles and browser.

**Флаг:** YauzaCTF{9a078583e7e201674015c721d7db1c001d6f1f}

# Important Information 1


PDF (образ памяти).
Пароль в виде картинки. 
У одного из объектов указан неверный параметр сжатия. 
Дамп оперативной памяти => ищем процессы (вспоминаем, какое ПО может открыть PDF) => ищем в дампе памяти процесса объекты PDF.
Флаг: YauzaCTF{Fl4t3_D3c0d3_F4ls3}

# Important Information 2


Браузер (триаж).
	
Нужно рассмотреть кэш браузера.
	
Флаг: YauzaCTF{sql1t3_recovery}


# Important Information 3

KeePass (образ памяти)

Получить пароль от учётки, сохранённой в KeePass. 

Найти по сигнатуре .kdbx файл. И посмотреть, что осталось в буфере обмена (там пароль от .kdbx базы).
	
Флаг: YauzaCTF{k33p44sssss}



