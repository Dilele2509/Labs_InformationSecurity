# 21110114, Le Thuy Tuong Vy
# Lab 1: Buffer Overflow

# Task 1: Stack smashing by memory overwritten
### 1.1. bof1.c
<span style="color:red"><b>Idea:</b></span> <b>The primary goal of the bof1.c program is to execute a buffer overflow attack to force the program to display the contents of the <span style="color:yellow">secretFunc()</span> function.</b> <br>
<!-- step 1 -->
<span style="color:red"><b>Step 1:</b></span> Compile the C file with the <span style="color:yellow">'gcc'</span> command, and you will receive a warning that the <span style="color:yellow">'gets()'</span> function is unsafe. This warning indicates a potential buffer overflow vulnerability that can be exploited.<br>
<img width="726" alt="bof1 1" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/bof1.1.png"><br>

<!-- step 2 -->
<span style="color:red"><b>Step 2:</b></span> Load the program into GDB and run it. First, input 200 <span style="color:yellow">'a'</span> characters followed by 4 <span style="color:yellow">'d'</span> characters. You'll see that the 4 <span style="color:yellow">'d'</span> characters are entirely within the EIP register. To successfully execute the buffer overflow attack on this program, print 204 <span style="color:yellow">'a'</span> characters followed by the address of the <span style="color:yellow">'secretFunc()'</span> function that you want to redirect to.<br>
<img width="726" alt="bof1 2" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/bof1.2.png"><br>

<!-- step 3 -->
<span style="color:red"><b>Step 3:</b></span> Find the address of the secretFunc() function.<br>
<img width="726" alt="bof1 3" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/bof1.3.png"><br>

<!-- step 4 -->
<span style="color:red"><b>Step 4:</b></span> Execute the code by creating a payload named “inputbof1,” which includes 204 <span style="color:yellow">'a'</span> characters followed by the address of the secretFunc() function.<br>
<img width="726" alt="bof1 4" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/bof1.4.png"><br>

### 1.2. bof2.c
<span style="color:red"><b>Idea:</b></span> <b>The purpose of the bof2.c code is to demonstrate a buffer overflow attack that makes the program output the message "Yeah! You win!" under specific conditions. To achieve this, two criteria must be met: first, a buffer overflow must occur, and second, the variable <span style="color:yellow">'check'</span> must hold the value <span style="color:yellow">'deadbeef'</span>.</b> <br>
<span style="color:red"><b>Step 1:</b></span> Compile the C file using <span style="color:yellow">'gcc'</span><br>
<span style="color:red"><b>Step 2:</b></span> When you input 40 <span style="color:yellow"><i>'a'</i></span> characters, they fit exactly within the buffer's allocated space.
<span style="color:red"><b>Step 3:</b></span> If you attempt to input 41 <span style="color:yellow"><i>'a'</i></span> characters afterwards, one <span style="color:yellow"><i>'a'</i></span> character will overwrite the <span style="color:yellow"><i>'check'</i></span> variable. This indicates a buffer overflow, where the extra <span style="color:yellow"><i>'a'</i></span> characters overwrite adjacent memory, specifically affecting the <span style="color:yellow"><i>'check'</i></span> variable.
<br>
<img width="726" alt="bof2.1" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/bof2.1.png"><br>

<span style="color:red"><b>Step 4:</b></span> To successfully perform the buffer overflow attack in bof2, simply input 40 <span style="color:yellow"><i>'a'</i></span> characters followed by inserting the string <span style="color:yellow"><i>'deadbeef'</i></span> into the <span style="color:yellow"><i>'check'</i></span> variable. This action will cause the program to output the line "Yeah! You win!”<br>
<img width="726" alt="bof2.1" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/bof2.2.png"><br>

## 1.3. bof3.c
<span style="color:red"><b>Idea:</b></span> <b>The main goal of the bof3.c program is to execute a buffer overflow attack, causing the program to display the content of the <spand style="color:yellow">shell()</spand> function. This is achieved by overflowing the buffer array and including the <spand style="color:yellow">shell()</spand> function address in the attack instructions. The program uses the fgets() function, which has a buffer overflow vulnerability.</b><br>

<span style="color:red"><b>Step 1:</b></span> Compile the program with gcc and load it into gdb. Run it initially before entering the necessary input.<br>

<span style="color:red"><b>Step 2:</b></span> Observe the buffer <span style="color:yellow">'buf[128]'</span>. You can exploit it by inputting 128 <span style="color;yellow">'a'</span> characters followed by 4 bytes of any characters to check for overflow into EIP. If 4 <span style="color:yellow">'d'</span> characters are inserted into EIP, the offset for the program will be 128 characters plus the address of the <span style="color:yellow">shell()</span> function.<br>

<span style="color:red"><b>Step 3:</b></span> Use gdb to locate the address of the <span style="color:yellow">shell()</span> function.<br>

<span style="color:red"><b>Step 4:</b></span> To complete the buffer overflow, input 128 <span style="color:yellow">'a'</span> characters followed by the address of the <span style="color:yellow">shell()</span> function. Create a payload file named <span style="color:yellow">"inputbof3"</span> with these contents. Use this payload file in gdb to run the program and achieve the desired result. This approach is commonly used to test buffer overflow vulnerabilities and automate the process.<br>

# Task 2: Code injection
## 2.1. Preparing shell code
## 2.3. Code injection