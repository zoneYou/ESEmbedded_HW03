HW03
===
This is the hw03 sample. Please follow the steps below.

# Build the Sample Program

1. Fork this repo to your own github account.

2. Clone the repo that you just forked.

3. Under the hw03 dir, use:

	* `make` to build.

	* `make clean` to clean the ouput files.

4. Extract `gnu-mcu-eclipse-qemu.zip` into hw03 dir. Under the path of hw03, start emulation with `make qemu`.

	See [Lecture 02 ─ Emulation with QEMU] for more details.

5. The sample is a minimal program for ARM Cortex-M4 devices, which enters `while(1);` after reset. Use gdb to get more details.

	See [ESEmbedded_HW02_Example] for knowing how to do the observation and how to use markdown for taking notes.

# Build Your Own Program

1. Edit main.c.

2. Make and run like the steps above.

3. Please avoid using hardware dependent C Standard library functions like `printf`, `malloc`, etc.

# HW03 Requirements

1. How do C functions pass and return parameters? Please describe the related standard used by the Application Binary Interface (ABI) for the ARM architecture.

2. Modify main.c to observe what you found.

3. You have to state how you designed the observation (code), and how you performed it.

	Just like how you did in HW02.

3. If there are any official data that define the rules, you can also use them as references.

4. Push your repo to your github. (Use .gitignore to exclude the output files like object files or executable files and the qemu bin folder)

[Lecture 02 ─ Emulation with QEMU]: http://www.nc.es.ncku.edu.tw/course/embedded/02/#Emulation-with-QEMU
[ESEmbedded_HW02_Example]: https://github.com/vwxyzjimmy/ESEmbedded_HW02_Example

--------------------

- [ ] **If you volunteer to give the presentation next week, check this.**

--------------------

**★★★ Please take your note here ★★★**

# 實驗內容

編寫`main.c`檔利用簡單的c code觀察程式運轉時,暫存器的呼叫與調用.

# 實驗步驟

1.編寫`main.c`檔

```
void reset_handler(void)
{
	int c0 = 0,c1 = 1,c2 = 2,c3 = 3;
 	int a = 3;
	int b = 6;
	c0=a+b;
	c0=b/c2;
	c0=b/c3;
	c0=b/c1;

	while (1)
		;
}
```
2.輸入`$make`進行編譯
```
sassembly of section .mytext:

00000000 <reset_handler-0x8>:
   0:	20000100 	andcs	r0, r0, r0, lsl #2
   4:	00000009 	andeq	r0, r0, r9

00000008 <reset_handler>:
   8:	b480      	push	{r7}
   a:	b087      	sub	sp, #28
   c:	af00      	add	r7, sp, #0
   e:	2300      	movs	r3, #0
  10:	617b      	str	r3, [r7, #20]
  12:	2301      	movs	r3, #1
  14:	613b      	str	r3, [r7, #16]
  16:	2302      	movs	r3, #2
  18:	60fb      	str	r3, [r7, #12]
  1a:	2303      	movs	r3, #3
  1c:	60bb      	str	r3, [r7, #8]
  1e:	2303      	movs	r3, #3
  20:	607b      	str	r3, [r7, #4]
  22:	2306      	movs	r3, #6
  24:	603b      	str	r3, [r7, #0]
  26:	687a      	ldr	r2, [r7, #4]
  28:	683b      	ldr	r3, [r7, #0]
  2a:	4413      	add	r3, r2
  2c:	617b      	str	r3, [r7, #20]
  2e:	683a      	ldr	r2, [r7, #0]
  30:	68fb      	ldr	r3, [r7, #12]
  32:	fb92 f3f3 	sdiv	r3, r2, r3
  36:	617b      	str	r3, [r7, #20]
  38:	683a      	ldr	r2, [r7, #0]
  3a:	68bb      	ldr	r3, [r7, #8]
  3c:	fb92 f3f3 	sdiv	r3, r2, r3
  40:	617b      	str	r3, [r7, #20]
  42:	683a      	ldr	r2, [r7, #0]
  44:	693b      	ldr	r3, [r7, #16]
  46:	fb92 f3f3 	sdiv	r3, r2, r3
  4a:	617b      	str	r3, [r7, #20]
  4c:	e7fe      	b.n	4c <reset_handler+0x44>
  4e:	bf00      	nop

Disassembly of section .comment:

00000000 <.comment>:
   0:	3a434347 	bcc	10d0d24 <reset_handler+0x10d0d1c>
   4:	35312820 	ldrcc	r2, [r1, #-2080]!	; 0xfffff7e0
   8:	392e343a 	stmdbcc	lr!, {r1, r3, r4, r5, sl, ip, sp}
   c:	732b332e 			; <UNDEFINED> instruction: 0x732b332e
  10:	33326e76 	teqcc	r2, #1888	; 0x760
  14:	37373131 			; <UNDEFINED> instruction: 0x37373131
  18:	2029312d 	eorcs	r3, r9, sp, lsr #2
  1c:	2e392e34 	mrccs	14, 1, r2, cr9, cr4, {1}
  20:	30322033 	eorscc	r2, r2, r3, lsr r0
  24:	35303531 	ldrcc	r3, [r0, #-1329]!	; 0xfffffacf
  28:	28203932 	stmdacs	r0!, {r1, r4, r5, r8, fp, ip, sp}
  2c:	72657270 	rsbvc	r7, r5, #112, 4
  30:	61656c65 	cmnvs	r5, r5, ror #24
  34:	00296573 	eoreq	r6, r9, r3, ror r5

Disassembly of section .ARM.attributes:

00000000 <.ARM.attributes>:
   0:	00003041 	andeq	r3, r0, r1, asr #32
   4:	61656100 	cmnvs	r5, r0, lsl #2
   8:	01006962 	tsteq	r0, r2, ror #18
   c:	00000026 	andeq	r0, r0, r6, lsr #32
  10:	726f4305 	rsbvc	r4, pc, #335544320	; 0x14000000
  14:	2d786574 	cfldr64cs	mvdx6, [r8, #-464]!	; 0xfffffe30
  18:	0600344d 	streq	r3, [r0], -sp, asr #8
  1c:	094d070d 	stmdbeq	sp, {r0, r2, r3, r8, r9, sl}^
  20:	14041202 	strne	r1, [r4], #-514	; 0xfffffdfe
  24:	17011501 	strne	r1, [r1, -r1, lsl #10]
  28:	1a011803 	bne	4603c <reset_handler+0x46034>
  2c:	22061e01 	andcs	r1, r6, #1, 28
  30:	Address 0x0000000000000030 is out of bounds.
```

3.利用`qemu`模擬觀察程式運作下狀態的改變

紀錄下每一行程式執行狀態並觀察

每執行一次並截圖觀察,已將圖片放置於資料夾"img-HW03"


# 實驗結果與討論

1.每次存入變數會放至r3暫存器在傳至r7

2.r7每存入一個變數，變數地址遞減4

3.運算結果主要存入r3，多變數計算才會使用到r2或以下

4.每次呼叫變數一律由`r7＃位置`提取並運算在放置到r3





