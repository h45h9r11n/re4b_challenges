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
```assembly
    
```
