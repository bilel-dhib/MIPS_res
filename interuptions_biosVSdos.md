# idk modules
avoid data misinterpreted as instructions

mov ah,2               ;Fonction d'affichage "du contenu du dl"  caractere par caractere

int 21h                ;Interruption relative à l'affichage


BIOS is basically:

hardware initialization code

reusable low-level routines in ROM

It saves you from programming devices directly.

Without BIOS:

poll keyboard controller

read scan codes from I/O port 60h

handle key release/press logic yourself

| Interrupt | Layer         |
| --------- | ------------- |
| `INT 10h` | BIOS video    |
| `INT 13h` | BIOS disk     |
| `INT 16h` | BIOS keyboard |
| `INT 21h` | DOS services  |

bios interupts are

These are firmware-level services.

and INT 21h is the main API of MS-DOS.

Function AH=09h means:

“Display a $-terminated string.”

Many embedded microcontroller systems have:

no BIOS
no OS

and programmers directly manipulate:

GPIO registers
display controllers
UARTs
timers

So “manual hardware control” is actually very common outside PCs.
`CPU + hardware docs??`

CPU alone
→ can execute instructions only

CPU + hardware docs
→ can control devices manually

CPU + BIOS
→ easier generic hardware access

CPU + BIOS + DOS
→ convenient operating environment

| Function (AH) | Meaning                     |
| ------------- | --------------------------- |
| 00h           | set video mode              |
| 02h           | set cursor position         |
| 09h           | write character & attribute |
| 0Eh           | teletype output             |
| 13h           | write string                |

