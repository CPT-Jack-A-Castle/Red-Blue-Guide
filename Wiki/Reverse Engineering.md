# RedTeam

## Build
- From binary to
    - Machine code (via hexdump)
    - Assembly Code (via Disassembler)
    - Source Code/High-Level Code (via Decompiler)

## JMP ESP (Jump Stack Pointer)

*How is "JMP ESP" presented in Assembler and Opcode?*

1. JMP ESP ( \xFF\xE4)
2. CALL ESP ( \xFF\xD4)
3. PUSH ESP; RET (\x54\xC3)

**Automatic Search for JMP ESP within example.exe**

- `/usr/share/metasploit-framework/modules/msfpescan -j ESP [example.exe]`

## File Signature/Magic Number

*First two bytes of binary header*: [List of signatures](https://en.wikipedia.org/wiki/List_of_file_signatures)

- Windows executable: 4D 5A
    - Exp: EXE | DLL | SYS | DRV | COM 
- ASCII:  MZ
- Base64: TV
- ELF: 7f 45 4c 46
- INDX ($I30): 49 4E 44 58
- PNG: 89 50 4E 47 0D 0A 1A 0A
- JPEG: FF D8 FF E0 / FF D8 FF EE / FF D8 FF E1

## Tools
- **ltrace**
    - Analyze library calls of an application
    - `ltrace -p [pid]`: Attach ltrace to running app with known process id
- **strace**
    - Analyze system calls of an application
    - `strace -p [pid]`: Attach strace to running app with known process id
- **GNU Debugger**
    - Set Intel Syntax: 
        - Permanent: `cd ~` + `nano .gdbini` + Enter: `set disassembly intel`
        - Temporarily (running gdb already): `set disassembly intel`
    - Common Commands:
        - `gdb ./binary`: start gdb with binary
        - `disass main` or `disass function_name`: show assembler code
        - `run` or `r`: start binary
        - `next` or `n`: move to next line (e.g. after your defined breakpoint)
        - `step` or `s`: step into function call and see its "inner life" 
        - `continue` or `c`: move from current location to next breakpoint (faster then "next")
        - `frame` or `f`: show frame (current code line)
        - `print` or `whatis` or `ptype`: (detailed) print of variable or datatype
        - `list`: show C source code if available
        - `info locals` or `info variables` or `info functions` or `info breakpoints` or `info registers` 
        - `break main` or `break *0x080483` or `b printf` or `break file.c:15` or `b`: set breakpoint (addresses start with *)
        - `delete breakpoints` or `disable`: delete or disable breakpoints
        - `backtrace`: Show backtrace
        - `set var = 3` or `print var = 3` or `set *(0x7ffffff)=7` or `set *(0x7ffffff)=0x61616161`: set value of variable manually
        - `set $eax`: set address of variable manually
        - `x/[Length][Format][Adress expression]`: Dump of elements
            - `x/4wb &i`
            - `x/25x $eax`
        - `p/d 0x7fffffffdfb8 - 0x7fffffffdf30`: Calculate difference between two memory addresses (e.g. to calculate buffer size)
            - `b leave`, move to this breakpoint and press `i f` to get EIP address
            - then press `x/20wx $esp` to check ESP 
        - `quit` or `q`: exit gdb
- **gcc**: GNU Compiler Collection
    - example.c --> program code in C
    - example.s --> interim data with assembler code
    - example.o --> object File
    - example --> compiled binary 
    - Options (extract):
      - `-Wall` = Warning all (Syntax/Logikfehler)
      - `-o` = output
      - `-g` = generate debug info
          - `-g0`: no debug info 
          - `-g1`: minimal debug info
          - `-g`: default debug info
          - `-g3`: maximal debug info
      - `-01` to `-03` = Code optimization (the more, the more compact)
      - `-save-temps` = no deletion of temporary data while compilation
      - `-m32` = compile 32bit binary on 64bit machine 
    - Example: `gcc -Wall example.c -o example` 
- **file**: show file type
    - `file [data]`
- **base64**: base64 encode/decode data and print to standard output
    - `echo "khdfkhfs==" | base64 -d` 
- **hexdump**: Display file in hexadecimal/decimal/ctal/ASCII
    - `hexdump -C [file] | head -n 15`
- **hexedit**: View and edit files in hexadecimal or in ASCII
- **xxd**: create/revert hexdump
    - `echo "0x70" | xxd -r -d`
    -  `xxd -s -0x30 file`: Print 3 lines (hex 0x30 bytes) from the end of file
    -  `xxd -l 120 -c 12 file`: Hexdump the first 120 bytes of file with 12 octets per line
- **binwalk**: firmware analysis tool
    - `binwalk --help`
    - `binwalk -E [file]`: Measure entropy of file
    - `binwalk -e [file]`: extract data
- **openssl**
    - `openssl help`
- **readelf**: 
    - `readelf -h [.bin]`: show ELF Headers
    - `readelf --segments [.bin] | ls`: show all segments (Data:RW, Code:RX)
    - `readelf --sections [.bin] | ls`: show all segments (Data:RW, Code:RX)
- **gdb**: Debugger
- **ldd**: 
    - `ldd $(which binary)`: prints shared objects/libraries required by specified [binary]
- **size**: 
    - `size [option] [file]`: Displays the sizes of sections inside binary files (size -A -x [file])
- **strings**: useful for searching strings in non-text file
    - `strings -n 6 [file]` show strings with at least six chars 
- **vimdiff**: compare binaries
    - `vimdiff file1.bin file2.bin`

## Assembler Code
See here: [Coding/Assembler](https://github.com/p-arrow/Red-Blue-Guide/blob/main/Coding/Assembler.md)

<br />

# BlueTeam
## Best Practice
Attackers need information about information control flow of an application in order to manipulate memory access and registers. Consequently, defenders have to prevent attackers from performing static code analysis