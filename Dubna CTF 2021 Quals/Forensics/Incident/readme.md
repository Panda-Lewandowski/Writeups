# Strange file
## Описание


На компьютере мошенника нашли файл с довольно странным содержанием. 
Был снят дамп оперативной памяти.
Эксперты Group-IB легко справляются с этой задачей, попробуй и ты!

Автор: Panda

## Решение

Проще всего данный таск решается с помощью volatility 2ой версии. Однако ключи из памяти, необходимые для решения, можно достать разными методами, например, bulk_extractor.

Итак, для начала узнаем, с какой опрационной системы снят дамп: 
```
$ python2 vol.py -f dump.raw imageinfo

Volatility Foundation Volatility Framework 2.6.1
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win7SP1x64, Win7SP0x64, Win2008R2SP0x64, Win2008R2SP1x64_24000, Win2008R2SP1x64_23418, Win2008R2SP1x64, Win7SP1x64_24000, Win7SP1x64_23418
                     AS Layer1 : WindowsAMD64PagedMemory (Kernel AS)
                     AS Layer2 : FileAddressSpace (/Users/pandas/Documents/Writeups/Dubna CTF 2021 Quals/forensics_incident/dump.raw)
                      PAE type : No PAE
                           DTB : 0x187000L
                          KDBG : 0xf80002a02110L
          Number of Processors : 1
     Image Type (Service Pack) : 1
                KPCR for CPU 0 : 0xfffff80002a03d00L
             KUSER_SHARED_DATA : 0xfffff78000000000L
           Image date and time : 2021-04-27 12:22:42 UTC+0000
     Image local date and time : 2021-04-27 05:22:42 -0700
```

Профили в поле `Suggested Profile(s)` расположены от более вероятного до менее вероятного, поэтому далее будем указывать профайл `Win7SP1x64`. Проверим, что за неизвестный jpg.

```
$ file 734px-Dys7sO8WwAIsL9V.jpg                                                                                   

734px-Dys7sO8WwAIsL9V.jpg: data
```

На JPEG это похоже меньше всего. Вероятнее всего данные зашифрованы. Посмотрим, что происходило внутри самой машины, когда был снят дамп.

```
$ python2 vol.py -f dump.raw --profile=Win7SP1x64 pstree             
Volatility Foundation Volatility Framework 2.6.1
Name                                                  Pid   PPid   Thds   Hnds Time
-------------------------------------------------- ------ ------ ------ ------ ----
 0xfffffa800181d060:wininit.exe                       388    340      3     76 2021-04-27 12:15:41 UTC+0000
. 0xfffffa800256b270:services.exe                     448    388     16    255 2021-04-27 12:15:42 UTC+0000
.. 0xfffffa80028cc060:svchost.exe                    1408    448     12    151 2021-04-27 12:15:43 UTC+0000
.. 0xfffffa8002788b10:svchost.exe                     704    448      8    281 2021-04-27 12:15:42 UTC+0000
.. 0xfffffa8002a9f600:VSSVC.exe                      2732    448      7    117 2021-04-27 12:15:51 UTC+0000
.. 0xfffffa80028a0b10:svchost.exe                     912    448     20    603 2021-04-27 12:15:42 UTC+0000
.. 0xfffffa8000e10b10:svchost.exe                    2992    448     13    355 2021-04-27 12:22:24 UTC+0000
.. 0xfffffa8002bdbb10:mscorsvw.exe                   1300    448      6     79 2021-04-27 12:22:23 UTC+0000
.. 0xfffffa800274f060:svchost.exe                    1520    448     18    235 2021-04-27 12:15:43 UTC+0000
.. 0xfffffa8002775b10:vm3dservice.ex                  680    448      4     46 2021-04-27 12:15:42 UTC+0000
.. 0xfffffa8001381430:taskhost.exe                   1216    448     10    163 2021-04-27 12:15:43 UTC+0000
.. 0xfffffa800074cb10:cygrunsrv.exe                  1728    448      6     99 2021-04-27 12:15:44 UTC+0000
... 0xfffffa8001853910:cygrunsrv.exe                  884   1728      0 ------ 2021-04-27 12:15:45 UTC+0000
.... 0xfffffa8001f444f0:sshd.exe                     1292    884      4     98 2021-04-27 12:15:45 UTC+0000
.. 0xfffffa80028c19c0:svchost.exe                     944    448     48   1017 2021-04-27 12:15:42 UTC+0000
.. 0xfffffa8002817b10:svchost.exe                     840    448     22    439 2021-04-27 12:15:42 UTC+0000
... 0xfffffa80027af060:dwm.exe                       1308    840      4     73 2021-04-27 12:15:43 UTC+0000
.. 0xfffffa800173d860:VGAuthService.                 1780    448      4     85 2021-04-27 12:15:44 UTC+0000
.. 0xfffffa8002a15b10:spoolsv.exe                    1084    448     15    283 2021-04-27 12:15:43 UTC+0000
.. 0xfffffa8000726060:mscorsvw.exe                    816    448      6     85 2021-04-27 12:22:23 UTC+0000
.. 0xfffffa8002896060:sppsvc.exe                     1760    448      4    150 2021-04-27 12:15:46 UTC+0000
.. 0xfffffa80020feb10:msdtc.exe                      2432    448     14    152 2021-04-27 12:15:50 UTC+0000
.. 0xfffffa8001fc8b10:dllhost.exe                    2132    448     21    187 2021-04-27 12:15:46 UTC+0000
.. 0xfffffa800115cb10:WmiApSrv.exe                    984    448      6    117 2021-04-27 12:16:07 UTC+0000
.. 0xfffffa800207ab10:dllhost.exe                    2256    448     18    204 2021-04-27 12:15:47 UTC+0000
.. 0xfffffa8000759b10:wlms.exe                       1888    448      4     47 2021-04-27 12:15:44 UTC+0000
.. 0xfffffa80028e2b10:svchost.exe                    1000    448      6    119 2021-04-27 12:15:42 UTC+0000
.. 0xfffffa8001864060:vmtoolsd.exe                   1852    448     12    292 2021-04-27 12:15:44 UTC+0000
.. 0xfffffa800273fb10:svchost.exe                     620    448     11    362 2021-04-27 12:15:42 UTC+0000
... 0xfffffa8001f8bb10:WmiPrvSE.exe                  2124    620     10    192 2021-04-27 12:15:46 UTC+0000
... 0xfffffa800113b060:WmiPrvSE.exe                  3064    620     15    317 2021-04-27 12:16:06 UTC+0000
.. 0xfffffa8002a49b10:svchost.exe                    1136    448     23    347 2021-04-27 12:15:43 UTC+0000
.. 0xfffffa80027c1870:svchost.exe                     756    448     21    454 2021-04-27 12:15:42 UTC+0000
.. 0xfffffa800216f410:SearchIndexer.                 2544    448     13    556 2021-04-27 12:15:50 UTC+0000
.. 0xfffffa800294db10:svchost.exe                     636    448     21    404 2021-04-27 12:15:43 UTC+0000
.. 0xfffffa8001e0c560:svchost.exe                    1824    448      6     98 2021-04-27 12:15:46 UTC+0000
. 0xfffffa8002591b10:lsass.exe                        456    388      7    594 2021-04-27 12:15:42 UTC+0000
. 0xfffffa8002667b10:lsm.exe                          464    388     10    146 2021-04-27 12:15:42 UTC+0000
 0xfffffa800244ab10:csrss.exe                         348    340      8    576 2021-04-27 12:15:41 UTC+0000
. 0xfffffa8001d78060:conhost.exe                     1372    348      2     33 2021-04-27 12:15:45 UTC+0000
 0xfffffa8002590600:winlogon.exe                      492    380      4    118 2021-04-27 12:15:42 UTC+0000
 0xfffffa80018153a0:csrss.exe                         400    380      9    212 2021-04-27 12:15:41 UTC+0000
. 0xfffffa8000f16060:conhost.exe                     2152    400      2     52 2021-04-27 12:22:40 UTC+0000
 0xfffffa8000634b10:System                              4      0     94    502 2021-04-27 12:15:41 UTC+0000
. 0xfffffa800184d040:smss.exe                         268      4      2     29 2021-04-27 12:15:41 UTC+0000
 0xfffffa80028a0060:explorer.exe                     1352   1296     40   1098 2021-04-27 12:15:43 UTC+0000
. 0xfffffa8001646500:vmtoolsd.exe                    1700   1352     10    180 2021-04-27 12:15:44 UTC+0000
. 0xfffffa8002a747b0:vm3dservice.ex                  1692   1352      3     49 2021-04-27 12:15:44 UTC+0000
. 0xfffffa8002c24b10:TrueCrypt.exe                   1972   1352      2     78 2021-04-27 12:16:09 UTC+0000
. 0xfffffa8000f1c060:DumpIt.exe                      1228   1352      1     26 2021-04-27 12:22:40 UTC+0000
```


Из необычного здесь только процесс `DumpIt.exe` и `TrueCrypt.exe`. Если первый процесс это просто программа для снятия дампов оперативной памяти, то второй представляет для нас интерес. В volatility2 есть встроенный плагин для вытаскивания информации о TrueCrypt. Попытаемся узнать сначала общую информацию. 

```
$ python  vol.py -f dump.raw --profile=Win7SP1x64 truecryptsummary

Volatility Foundation Volatility Framework 2.6.1
Process              TrueCrypt.exe at 0xfffffa8002c24b10 pid 1972
Service              truecrypt state SERVICE_RUNNING
Kernel Module        truecrypt.sys at 0xfffff88003638000 - 0xfffff88003679000
Symbolic Link        E: -> \Device\TrueCryptVolumeE mounted 2021-04-27 12:22:13 UTC+0000
Symbolic Link        E: -> \Device\TrueCryptVolumeE mounted 2021-04-27 12:22:13 UTC+0000
Symbolic Link        Volume{a8f3ffc0-a74f-11eb-b3b0-000c2924e859} -> \Device\TrueCryptVolumeE mounted 2021-04-27 12:16:33 UTC+0000
File Object          \Device\TrueCryptVolumeE\ at 0x1ecb17b0
File Object          \Device\TrueCryptVolumeE\ at 0x1ed11510
Driver               \Driver\truecrypt at 0x1f08b420 range 0xfffff88003638000 - 0xfffff88003679000
Device               TrueCryptVolumeE at 0xfffffa800073c860 type FILE_DEVICE_DISK
Container            Path: \??\C:\Users\IEUser\Desktop\734px-Dys7sO8WwAIsL9V.jpg
Device               TrueCrypt at 0xfffffa800148b1f0 type FILE_DEVICE_UNKNOWN
```

Видим, что на момент снятия дампа TrueCrypt запущен, следовательно ключи могли остаться в памяти. 

```
$ python2 vol.py -f dump.raw --profile=Win7SP1x64 truecryptmaster -D .

Volatility Foundation Volatility Framework 2.6.1
Container: \??\C:\Users\IEUser\Desktop\734px-Dys7sO8WwAIsL9V.jpg
Hidden Volume: No
Removable: No
Read Only: No
Disk Length: 1835008 (bytes)
Host Length: 2097152 (bytes)
Encryption Algorithm: AES
Mode: XTS
Master Key
0xfffffa8000e081a8  3b 3b 44 b2 80 17 e8 c2 62 72 31 53 2b ec 3b e2   ;;D.....br1S+.;.
0xfffffa8000e081b8  a3 eb 7c 4d 32 8e 27 2a d9 23 a9 d3 4d 8e 86 be   ..|M2.'*.#..M...
0xfffffa8000e081c8  82 59 b1 07 24 f8 8e 07 c2 40 26 3f 67 11 7f 6a   .Y..$....@&?g..j
0xfffffa8000e081d8  e4 6b 57 8d 97 37 0e f3 ec 78 6b 62 f3 da 96 b9   .kW..7...xkb....
Dumped 64 bytes to ./0xfffffa8000e081a8_master.key
```

Ключ к тому TrueCrypt помещен в файл [0xfffffa8000e081a8_master.key](./0xfffffa8000e081a8_master.key), остается только примонтировать и расшифровать том. Проще всего это сделать при помощи утилиты [MKDecrypt](https://github.com/AmNe5iA/MKDecrypt).

```
sudo python MKDecrypt.py -m /mnt/ 734px-Dys7sO8WwAIsL9V.jpg -X 0xfffffa8000e081a8_master.key
```

В примонтированном томе нас ждет один текстовый файлик с флагом. 

## Флаг 

`dubna{true_crypt_is_not_safety}`