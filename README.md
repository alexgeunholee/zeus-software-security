Zeus Software Defender Technology (“Zeus”) is a tool for hardening C and C++ programs to provide shields for protecting against cyber security attacks. 

SEI (Carnegie Mellon University Software Engineering Institute) operates the CERT Division that is supported by the US government and which is the world best in software security research. According to the CERT C/C++ Coding Standard for Secure Coding in C/C++, all code pointers must be protected in any C/C++ based software. 

Zeus uses the program counter encoding technique to protect pointers (return addresses, function pointers), which control data for address of an instruction on memory, implemented into C and C++ Compilers to reduce software vulnerabilities effectively without increasing execution time for protective measures, and thus, to provide secure software through hardening executable binaries.

Buffer Overflow Attacks overwrite a buffer in a computer memory beyond buffer boundary so that a nearby code pointer is set to an address that attackers desire: The control-flow diverts to the attacker’s destination when the pointer is de-referenced. From the attacker’s destination, illegal control-flow begins that results in allowing the attacker to execute arbitrary code or leaking of critical information. Zeus dynamically re-encrypts code pointers using code for encryption inserted at compile time for obfuscation of code pointers to mitigate both control-flow interception and leakage of code pointer values. Zeus re-encrypts code pointers in runtime before or after risky operations. 

Zeus can successfully mitigate real world cyber security attacks reported in CVE (Common Vulnerabilities and Exposures). Followings illustrate some demos regarding Zeus defense againt real world cyber security attacks reported in CVE.

Zeus Defense Demo against Code Pointer Attack I

Link: https://www.youtube.com/watch?v=1HL8bQ21_qs&feature=youtu.be

CVE-2013-2028: Nginx Web Server stack based buffer overflow case regarding leaking critical information through Blind ROP (Return Oriented Programming) attack. 
Ref. Bittau, Andrea, et al. "Hacking blind." 2014 IEEE Symposium on Security and Privacy. IEEE, 2014.

1. An attacker injects a payload that consists of an attack script for information leak into target stack.
2. The attacker starts the attack by executing the attack script.
3. Return address in target stack leaks to attacker. The attacker even can find return address form the encrypted return 
address using incremental byte reading. 
4. The attacker crafts a new payload using the leaked return address.
5. The attacker injects the new payload into target stack.
6. Target’s memory contents including code leaks to attacker.
7. The attacker analyzes the memory contents to build an ROP chain.
8. Attacker launches the attack using the ROP chain. During each attack, the Web Server repeats crash and restart again and again. 

Under Zeus protection, the leaked return address in step 3 cannot be used for further leakage because the target program  has already reencrypted the return address using a new encryption key.
Thus, the new attack in step 5 fails. 

Zeus Defense Demo against Code Pointer Attack II

Link: https://www.youtube.com/watch?v=FEKQMRrgvws&feature=youtu.be

CVE-2013-2028: Nginx Web Server stack based buffer overflow case regarding arbitrary code execution through ROP (Return Oriented Programming)
Ref. https://www.rapid7.com/resources/rop-exploit-explained

1. An attacker injects a payload that consists of an attack script.
2. The script starts with building an exploit that is made of a shellcode and a ROP chain.
3. The attacker injects an exploit into the server's buffer to overwrite a return address into the starting address of the ROP chain. 
4. Gadgets on the ROP chain are executed successively. A gadget is a set of successive instructions ending with return instruction.
5. Attacker executes the shellcode to perform arbitrary code execution.

Because return addresses are encrypted with a secret key when it is stored, return addresses should be decrypted with the secret key to corrupt the return addresses.
Thus, the attacker fails to execute the shellcode. 
Dynamic reencryption prevents the return address from overwritten to the starting address of the ROP chain.
Thus, the attack has failed to execute the ROP chain and could not open the shellcode.

Followings show some Zesu patents.

US8583939: Method and apparatus for securing indirect function calls by using program counter encoding
Abstract: A method for securing indirect function calls by using program counter encoding is provided. The method includes inserting a decoding code for an address of a library function stored in a GOT (Global Offset Table) entry into a PLT (Procedure Linkage Table) entry when an object file is built; generating an encoding key corresponding to the decoding code; and encoding the GOT entry corresponding to the library function by using the encoding key when program execution begins.

Link: https://patents.google.com/patent/US8583939B2

US10579806: Systems and methods for dynamic reencryption of code pointers

Abstract: Present disclosure provides the system and method for protecting the control-flow of a computer program against manipulation and leak of code pointers during program execution. The system includes a memory that a computer program is loaded onto and a processor which executes the computer program for protecting the control-flow of a program against manipulation and leak of code pointers during program execution. The method includes providing a shadow stack for each process and thread of the computer program in a thread local storage (TLS). Each code pointer is encrypted with the corresponding encryption key, the pair with a global key is encrypted, and reencryption of the code pointer at runtime is performed. The performing the reencryption of the code pointer includes renewing the corresponding encryption key in the shadow stack, and renewing the encryption state of the code pointer with a renewed encryption key when the computer program enters a code region vulnerable to a memory corruption or leakattack, such that one or more renewed encryption keys govern one or more corresponding code pointers through encryption while changing before the control-flow proceeds into the vulnerable region.

Link: https://patents.google.com/patent/US10579806B1

US20200076593: Systems and methods for encryption of virtual function table pointers

Abstract: The present disclosure presents systems and methods for virtual function table pointer encryption. Specifically, the systems and methods prevent outside attacks by encrypting the virtual function table pointers and further focus on encryption and decryption using keys differing among classes. The system includes a control unit, a memory management unit, a memory unit, a random key generation unit and a key storage unit. The control unit issues commands generating a key for encryption of the virtual function table pointer. The memory management unit generates a class ID from the class name. The memory unit stores the class name and the generated ID in a class ID table. The random key generation unit receives a command and generates an encryption key, and the key storage unit stores the class ID transmitted from the memory unit and the encryption key transmitted from the random key generation unit in the key storage unit

Link: https://patents.google.com/patent/US20200076593A1
