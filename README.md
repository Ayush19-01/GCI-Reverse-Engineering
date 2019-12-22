# GCI-Reverse-Engineering

### Debugger used: GDB

## Cracking the first file(1stcrackme)
As soon as I got the binary files I decided I would run the files through the strings function and easily crack the hidden passcodes.

So that's how I started with the first file, I ran it through the string's function and i got a list of keywords as follows:
![alt text](https://github.com/Ayush19-01/GCI-Reverse-Engineering/blob/master/resources/Screenshot%20from%202019-12-22%2013-59-36.png)

![alt text](https://github.com/Ayush19-01/GCI-Reverse-Engineering/blob/master/resources/Screenshot%20from%202019-12-22%2014-00-05.png)

![alt text](https://github.com/Ayush19-01/GCI-Reverse-Engineering/blob/master/resources/Screenshot%20from%202019-12-22%2014-00-10.png)

As it can be seen keyword `FEDORAGCIPASSEASY` was an obvious choice, but the twist was that the file had three passwords.

Then I noticed that under the paraphrase `Enter Password` there were three strings: `FEDORAGCIPASSEASY` , `0x1337` , `0x133337`.

I tried entering them and voila! Got success in all three.

![alt text](https://github.com/Ayush19-01/GCI-Reverse-Engineering/blob/master/resources/Screenshot%20from%202019-12-22%2014-20-21.png)

## Cracking the second file(2ndcrackme)

At this moment I was pretty confident that I can crack all the binary files with strings method and that's what I did.

I ran the second file through the string method and got the following output:

![alt text](https://github.com/Ayush19-01/GCI-Reverse-Engineering/blob/master/resources/Screenshot%20from%202019-12-22%2019-21-05.png)

![alt text](https://github.com/Ayush19-01/GCI-Reverse-Engineering/blob/master/resources/Screenshot%20from%202019-12-22%2019-31-44.png)

![alt text](https://github.com/Ayush19-01/GCI-Reverse-Engineering/blob/master/resources/Screenshot%20from%202019-12-22%2019-31-49.png)

The list of keyword's did not satisfy me this time, as i found out that there was no `Enter password` but only a placeholder.

After this two things were clear, I got to know that password must be entered while executing the binary file as given in the usage
and secondly the passwords were no longer going to be in the string format anymore.

That's when i used the GDB debugger on the second file as follows:

I had set the first breakpoint at starting by the command `(gdb) break *main` then I ran the code `run password` to get:

![alt text](https://github.com/Ayush19-01/GCI-Reverse-Engineering/blob/master/resources/Screenshot%20from%202019-12-22%2019-45-12.png)

After that i entered the disassemnle command `disass main` to get:

![alt text](https://github.com/Ayush19-01/GCI-Reverse-Engineering/blob/master/resources/Screenshot%20from%202019-12-22%2019-45-54.png)

I called the `info registers` and hit trialed all the values through the `x/s $ %s` method

After that there was key with value=0x7ffffffedf82 registered as rsi

After applying the `x/s rsi` i got the value at the address (0x7fffffffde28) as "FEd0raGCIt@sk"

Which after executing was the password!

![alt text](https://github.com/Ayush19-01/GCI-Reverse-Engineering/blob/master/resources/Screenshot%20from%202019-12-22%2018-40-05.png)

## Cracking the third file(3rdcrackme)

So, at this point it was obvious that the third password couldn't be extracted through the strings method, but i still ran it through the string method
just to know the usage of the file while being executed.

It gave me the following result:

![alt text](https://github.com/Ayush19-01/GCI-Reverse-Engineering/blob/master/resources/Screenshot%20from%202019-12-22%2021-00-49.png)

![alt text](https://github.com/Ayush19-01/GCI-Reverse-Engineering/blob/master/resources/Screenshot%20from%202019-12-22%2021-00-54.png)

![alt text](https://github.com/Ayush19-01/GCI-Reverse-Engineering/blob/master/resources/Screenshot%20from%202019-12-22%2021-01-03.png)

And man it bamboozled me, I literally convinced myself that the password was `FEDORAPASSWORDGCI!` and it wasn't.

But one thing is clear that the usage of this password is like the first one and the method of cracking maybe like the second one.

So next, I run the `3rdccrackme` file in the gdb format in the same manner as the second file.

I had set the first breakpoint at starting by the command `(gdb) break *main` then I ran the code `run password` to get:

![alt text](https://github.com/Ayush19-01/GCI-Reverse-Engineering/blob/master/resources/Screenshot%20from%202019-12-22%2021-20-35.png)

After that i entered the disassemnle command `disass main` to get:

![alt text](https://github.com/Ayush19-01/GCI-Reverse-Engineering/blob/master/resources/Screenshot%20from%202019-12-22%2021-21-26.png)
In this, with hard-coded password using memcmp at address <+109>. If our password is correct, then it jumps to <+125>, if not it sets [rbp-0x4] to 1 and continue execution. We can set our breakpoint at this memcmp using break *0x0000000008001214 and continue execution. When execution hit the breakpoint, we can check value of rsi.

We got the value of rsi as 0x7ffffffedf80: "00g61@k00land1514cel!", which is the password

![alt text](https://github.com/Ayush19-01/GCI-Reverse-Engineering/blob/master/resources/Screenshot%20from%202019-12-22%2018-45-53.png)

___Thank You for reading___

Acknowledgement
  GDB documentation
