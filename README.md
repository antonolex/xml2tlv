# xml2tlv

Convert xml-file to tlv-file. Test.

The input xml-file must be place in the directory of the executable.

Run exe-file, enter names of xml- and tlv-file and get result.

Allowed structure of input xml-file:

File must contain one root element with child elements inside.

Example:

![image](https://user-images.githubusercontent.com/85846871/160727764-4059ff6d-81bc-4779-9221-e55876636126.png)

Output tlv-file used next structure of fields:

Type - 1 byte 0x00 - hex 0x01 - string

Length - 1 byte - size of the value field in bytes

Value - variable-sized series of bytes which contains data for this part of the message


Necessary files to run converter:

xml2tlv.exe

zlib1.dll

libxml2.dll

iconv.dll
