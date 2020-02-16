https://www.reddit.com/r/hackintosh/comments/den28t/whats_new_in_macos_catalina/

catalina problem

https://khronokernel.github.io/EC-fix-guide/laptop-ec.html

`PNP0C09` is `EC0`

apply fix:
Comment: `change EC0 to EC`
Find*[HEX]: `4543305f`
Replace[HEX]: `45435f5f`

!!!как достать dsdt

1. скачать ubuntu
https://ubuntu.com/tutorials/tutorial-create-a-usb-stick-on-macos#1-overview
2. go files sys/firmware/acpi/tables
3. extract
https://mackonsti.wordpress.com/2012/02/13/extract-dsdt-using-ubuntu-live-cd/

```
cd ~/Desktop
sudo cat /sys/firmware/acpi/tables/DSDT > DSDT.aml
```

!!!где взять refs
https://github.com/hungptse/hackintosh-dsdt


!!! собрать все файлы в папку в виде DSDT.aml, SSDT1.aml..., refs.txt

!!! как разобрать dsdt файлы

1. скопировать iasl в /usr/bin
```
cd folder_with_iasl
sudo cp iasl /usr/bin
```

2. go to folder with dsdt files

iasl -da -dl -fe refs.txt *.aml

!!!
fix DSDT

problem:
- invalid object type for reserved name (_pld found buffer package required)

how to fix:
add patch to Maciasl
name "Fix _PLD Buffer/Package Error"
from "https://raw.githubusercontent.com/RehabMan/Laptop-DSDT-Patch/master"

problem:
- non-hex letters must be upper case (pnp0c14)

how to fix:
change `pnp0c14` to `PNP0C14`

problem:
- not all control paths return a value

how to fix:
add `Return(Zero)` at the end of function

problem:
- How to fix DSDT ResourceTag larger than Field

how to fix:
- change Dword to Qword

problem:
- result is not used, possible operator timeout will be missed

how to fix:
change `0x0FFF` to `0xFFFF`

problem:
- switch expression is not a static Integer/Buffer/String data type, defaulting to Integer
change `Switch (Arg0)` to `Switch (ToInteger (Arg0))`

problem:
- multiple types (device object requires either a _hid or _adr but not both)
how to fix:
remove line `Name (_ADR, Zero) // _ADR: Address`

problem:
- Legacy Processor() keyword detected. Use Device()

problem:
- unexpected `PARSE_PACKAGE`, unespected `{`
how to fix: 
apply patch `Remove _PSS placeholders`
// прописать usb порты
1. заменить `EHC1` на `EH01`
2. заменить `EHC2` на `EH02`

