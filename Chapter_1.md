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
destTime: Pointer to the storage location for time.
Returns the time as seconds elapsed since midnight, January 1, 1970, or -1 in the case of an error.
```
> time is a wrapper for _time64 and time_t is, by default, equivalent to __time64_t. If you need to force the compiler to interpret time_t as the old 32-bit time_t, you can define _USE_32BIT_TIME_T. This is not recommended because your application may fail after January 18, 2038; the use of this macro is not allowed on 64-bit platforms.
