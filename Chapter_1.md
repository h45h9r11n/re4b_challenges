# Chapter 1
## #48
#### What does this win32-function do?
```assembly
main:
    push 0xFFFFFFFF
    call MessageBeep
    xor  eax,eax
    retn
```

#### MessageBeep function
0xFFFFFFFF	A simple beep. If the sound card is not available, the sound is generated using the speaker
```C
MessageBeep(0xFFFFFFFF);
```

## #49
```assembly
main:
        pushq   %rbp
        movq    %rsp, %rbp
        movl    $2, %edi
        call    sleep
        popq    %rbp
        ret
```
```C
sleep(2);
```

## #52
```assembly
$SG3103	DB	'%d', 0aH, 00H

_main	PROC
	push	0
	call	DWORD PTR __imp___time64
	push	edx
	push	eax
	push	OFFSET $SG3103 ; '%d'
	call	DWORD PTR __imp__printf
	add	esp, 16
	xor	eax, eax
	ret	0
_main	ENDP
```
```C
__time64_t _time64( __time64_t *destTime );
```
> destTime: Pointer to the storage location for time.
> Returns the time as seconds elapsed since midnight, January 1, 1970, or -1 in the case of an error.
> time is a wrapper for _time64 and time_t is, by default, equivalent to __time64_t. If you need to force the compiler to interpret time_t as the old 32-bit time_t, you can define _USE_32BIT_TIME_T. This is not recommended because your application may fail after January 18, 2038; the use of this macro is not allowed on 64-bit platforms.

## #53
This code, compiled in Linux x86-64 using GCC is crashing while execution (segmentation fault). It's also crashed if compiled by MinGW for win32. However, it works in Windows environment if compiled by MSVC 2010 x86. Why?
```C
#include <string.h>
#include <stdio.h>

void alter_string(char *s)
{
        strcpy (s, "Goodbye!");
        printf ("Result: %s\n", s);
};

int main()
{
        alter_string ("Hello, world!\n");
};
```
 GCC stores string literals in read-only memory (.rodata section) when the program is run.
 
 ## #59
 What does this code do?
 ```asm
 _a$ = 8
_f	PROC
	mov	ecx, DWORD PTR _a$[esp-4]	;ecx = a
	lea	eax, DWORD PTR [ecx*8]		;eax = ecx*8 = a*8
	sub	eax, ecx			;eax = ecx - eax = 7*a
	ret	0
_f	ENDP
 ```
 
 ## #62
 ```asm
 _a$ = 8        ; size = 4
_b$ = 12       ; size = 4
_c$ = 16       ; size = 4
?s@@YAXPAN00@Z PROC     ; s, COMDAT
    mov    eax, DWORD PTR _b$[esp-4]			;eax = addr of b[]
    mov    ecx, DWORD PTR _a$[esp-4]			;ecx = addr of a[]
    mov    edx, DWORD PTR _c$[esp-4]			;edx = addr of c[]
    push   esi
    push   edi
    sub    ecx, eax					;ecx = x
    sub    edx, eax					;edx = y
    mov    edi, 200     ; 000000c8H			;edi = 200
$LL6@s:
    push   100          ; 00000064H
    pop    esi						;esi = 100
$LL3@s:
    fld    QWORD PTR [ecx+eax]				;st0 = b[x]
    fadd   QWORD PTR [eax]				;st0 = b[x] + b[0]
    fstp   QWORD PTR [edx+eax]				;b[y] = b[x] + b[0]
    add    eax, 8					;
    dec    esi						;esi--
    jne    SHORT $LL3@s
    dec    edi						;edi--
    jne    SHORT $LL6@s
    pop    edi
    pop    esi
    ret    0
?s@@YAXPAN00@Z ENDP     ; s
 ```
 
 ```c
 void f(float *a, float *b, float *c)
{
    long x = b - a;
    long y = b - c;
    for (int i = 200; i > 0; i--){
        for (int j = 100; j > 0; j--){
            b[y] = b[x] + b[0];
            b += 8;
        }
    }
}
 ```
 
 
