***MOV*** works by default with segment ***DS***

it's possible to force segment

adressage par:  registre,immediate(data),direct.
adressing mode:
<b>
- Register Mode 
- Immediate Mode
- Displacement or Direct Mode 
</b>

Remarque : Dans le cas de l’adressage **immédiat** de la mémoire,


il faut preciser byte or word (size of data)

# Register Indirect Mode:

In this type of addressing mode the effective address is directly given in the instruction as displacement.
## adressage basee:

l’offset est contenu dans un registre de base **BX** ou **BP**.

`mov ax,[bx]--> DS`

`mov ax,[bp]--> SS`

## adressage indexee:
offset in SI and DI
default **segment:** DS

**Remarque** :une valeur constante peut éventuellement être ajoutée aux registres de base ou d’index pour obtenir l’offset.

`mov [si+100H],ax`

## Adressage basé et indexé :
l’offset  is sum of base register and index register+(possible constants).

` mov ah,[bx+si+100H]`

note that 8086 works in **little endian.**

## MUL and DIV

**Multiplication : MUL** operand,  
where operand is a  register or  memory cell.

always use register A
```
if operand size=1byte:
    result in AX {16bits}
    result=operand*AL
else:
    result in (DX,AX) {32bits}
    2 highest bytes in DX the rest in AX
    result= operand*AX 
```
**DIVISION : DIV** operand,  
where operand is a  register or  memory cell.

always use register A
x=r+q*operand
```
if operand size=1byte:
    x=AX
    r=AH,q=AL
else:
    x=(DX,AX)
    r=DX,q=AX
    2 highest bytes in DX the rest in AX
    result= operand*AX 
```
example:
```
MOV AX, 1000h
MOV DX, 0      ; must clear DX!
DIV BX
```
# Decalage et rotations:
Dans les **décalages**, les bits qui sont déplacés sont remplacés par des **zéros**.

Dans les **rotations**, les bits déplacés dans un sens sont **réinjectés** de l’autre coté du mot.

there is decalage:
- logique
- arithmetique

## decalage logique :

the **last** bit shifted is in CF flag(carry flag)

**shr** operand,**n** 

**shl** operand,**n** 

``` 
for shift instructions (SHR, SHL, SAR, etc.)
The shift count can be:
- Immediate (constant)
- CL register only
```
## decalage arithmetique:
SAR:

sign bit(**MSB**) is reinjected in the same place 


```asm
mov al,01001011B
SAR al,2
``` 
## SAR intermediate results

| Operation     | 01001011B (0x4B) | 11001011B (0xCB) |
|---------------|------------------|------------------|
| Initial value | **01001011**         | **11001011**         |
| SAR al, 1     | 0**0100101**         | 1**1100101**         |
| SAR al, 2     | 00**010010**         | 1**1110010**         |


be aware ***SAL*** is just normal ***SHL*** i didnt find any difference

## ROtation:

(**ROR/ROL**) operand,n
rotate bits in direction right /left and copies the value of shited out bit in **CF**

**(RCR/RCL)** through carry(**CF**)
- reinjects the **previous** value of **CF** int the **""vacant"** bit
- copies the shifted out bit in **CF**  

# JUMPS:
- JG for signed 
- JA for unsigned
## Conditional Jumps (8086)

| Instruction | Meaning (signed)        | Condition (flags)        |
|------------|------------------------|--------------------------|
| JZ / JE    | Jump if zero / equal   | ZF = 1                   |
| JNZ / JNE  | Jump if not zero       | ZF = 0                   |
| JS         | Jump if negative       | SF = 1                   |
| JNS        | Jump if positive       | SF = 0                   |
| JG         | Jump if greater        | ZF = 0 AND SF = OF       |
| JL         | Jump if less           | SF ≠ OF                  |
| JGE        | Jump if ≥              | SF = OF                  |
| JLE        | Jump if ≤              | ZF = 1 OR SF ≠ OF        |
| JC         | Jump if carry          | CF = 1                   |
| JNC        | Jump if no carry       | CF = 0                   |
| JO         | Jump if overflow       | OF = 1                   |
| JNO        | Jump if no overflow    | OF = 0                   |





# Interuptions:
## output
### for printing string ending with '$'
```asm
mov ah,09h   
int 21h
```

`it prints the string with offset dx(this is DOS interupt)`

### for char:
```asm
mov ah,2
int 21h
```
`it prints the char with ascii code located in dl(this is DOS interupt)`