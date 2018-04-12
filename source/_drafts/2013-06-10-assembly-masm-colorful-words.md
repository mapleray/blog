---
title: 汇编显示彩色文字
tags:
  - assembly
permalink: assembly-masm-colorful-words
categories:
  - Programming
date: 2013-06-10 00:00:00
---


照着书上的思路，参考了一些答案，写出这个汇编小程序，显示出彩色的字，还真的激动了很久。
因为学习C就一直面对那个黑黑的dos窗口，想不到汇编竟然能直接就显示出来~~    
        
用`debug`载入，然后`-g 4c`运行到`int 21h`即可显示

    :::assembly
    assume cs:code

    data segment
        db 'welcome to masm!'
    data ends

    code segment

    start:
        mov ax,data
        mov ds,ax
        mov ax,0b800h
        mov es,ax
        mov si,0
        mov di,10*160+80
        mov cx,16

    s1:
        mov al,ds:[si]
        mov ah,00000010B
        mov es:[di],ax
        inc si
        inc di 
        inc di 
        loop s1
        mov si,0 
        mov di,11*160+80
        mov cx,16

    s2:
        mov al,ds:[si] 
        mov ah,00100100B
        mov es:[di],ax 
        inc si 
        inc di 
        inc di 
        loop s2
        mov si,0 
        mov di,12*160+80
        mov cx,16

    s3:
        mov al,ds:[si] 
        mov ah,01110001B
        mov es:[di],ax 
        inc si 
        inc di 
        inc di 
        loop s3
        mov ax,4c00h 
        int 21h

    code ends
    end start


![masm](/images/welcome-to-masm.jpg)
