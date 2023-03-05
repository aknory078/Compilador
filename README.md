# Compilador
Trabalho final
Introduction
The goal of this project is to develop a program that can take machine code as input and output human-readable code. This involves disassembling the binary file and converting it into assembly code. We will be using the BFD (Binary File Descriptor) library to do this.

Step 1: Installing BFD
The first step in creating our machine code translator is to install the BFD library. We can do this by installing the "binutils-dev" package on Ubuntu using the following command:
sudo apt-get install binutils-dev

Step 2: Setting up the code
The next step is to set up our C code to use the BFD library. We start by including the necessary headers:
#include <bfd.h>
#include <dis-asm.h>
Next, we define a function that will take the name of the binary file as input and return a BFD object:
bfd* open_bfd(char* filename) {
    bfd* abfd = bfd_openr(filename, NULL);
    if (!abfd) {
        fprintf(stderr, "Failed to open file: %s\n", filename);
        return NULL;
    }
    if (!bfd_check_format(abfd, bfd_object)) {
        fprintf(stderr, "File format is not recognized: %s\n", filename);
        bfd_close(abfd);
        return NULL;
    }
    return abfd;
}

This function opens the binary file and checks its format to make sure it is recognized by BFD.

 
Step 3: Disassembling the binary file

Once we have a BFD object, we can start disassembling the binary file. We do this by iterating over all of the sections in the binary file and disassembling any code sections we find. We also print out the disassembled code to the console.

void disassemble(bfd* abfd) {
    for (asection *section = abfd->sections; section; section = section->next) {
        if (section->flags & SEC_CODE) {
            printf("Disassembling section: %s\n", section->name);
            bfd_vma vma = section->vma;
            while (vma < section->vma + section->size) {
                disassemble_insn(abfd, section, vma);
                vma += get_insn_length(abfd, section, vma);
            }
        }
    }
}

This function loops through all of the sections in the binary file and checks if each section is a code section. If it is a code section, it disassembles the section by calling the disassemble_insn() function.

Step 4: Converting the disassembled code into C

After disassembling the binary file, we have the assembly code of the program in human-readable form. However, the assembly code is still far from being easily readable and understandable by most people, especially those without a background in computer architecture.

To make the code more understandable, we need to convert it into a higher-level programming language. In this case, we will convert it into the C programming language.

We can use regular expressions to parse the assembly code and generate C code. We will start by defining regular expressions for each type of instruction. For example, we can define a regular expression for a move instruction that matches lines such as "movl %eax, %ebx" or "movl $0x12345678, %eax". Similarly, we can define regular expressions for other instructions like add, sub, jmp, etc.

Once we have defined the regular expressions for the instructions, we can use them to generate C code that performs the same operations as the assembly code. We can also define variables in the C code to represent registers and memory locations used in the assembly code.

For example, the assembly code "movl $0x12345678, %eax" can be converted to the following C code:

unsigned int eax = 0x12345678;

Similarly, the assembly code "addl %eax, %ebx" can be converted to the following C code:

ebx += eax;

We can use the same approach to convert all the instructions in the assembly code into equivalent C code. Once we have converted all the instructions, we will have a C program that performs the same operations as the original binary file.

It is important to note that the resulting C code may not be identical to the original source code that was used to generate the binary file. This is because the original source code may have been optimized by the compiler, and some of the optimizations may not be easily replicable in C.

Step 5: Writing the code for machine language translator

After completing the setup, the next step was to write the code for the machine language translator. This involved using the bfd and dis-asm libraries to disassemble the binary into human-readable assembly instructions, which could then be modified into C code.

The code started by opening the binary file using bfd_openr() function and checking if it was successfully opened. Next, we iterated through the sections of the binary using asection structure and checked if the section was a code section using the SEC_CODE flag. This helped us to disassemble only the code sections of the binary.

We then used the bfd_get_section_contents() function to get the contents of the section and bfd_disassemble() function to disassemble each instruction. The disassembled instruction was stored in a buffer which was then used to create a mnemonic string. The mnemonic string was then analyzed to determine the type of instruction and the operands used.

Finally, we used the generated mnemonic and operands to generate C code for each instruction. The generated C code was written to a file for future reference.

The implementation of the machine language translator used a variety of methods and tools.

The program was written in C, a low-level programming language that is suitable for system programming, and can directly access hardware resources. The C language was chosen for its ability to work with binary data and its efficient use of system resources.

To read and process binary files, the program used the Binary File Descriptor (BFD) library, which is part of the GNU Binutils project. BFD provides a standardized way of accessing binary files and can handle a wide variety of formats.

To disassemble the machine language instructions in the binary file, the program used the disassembler library included in BFD. This library is able to parse machine instructions and convert them into human-readable assembly language code.

Once the machine language instructions had been disassembled, the program used a series of regular expressions to parse the assembly code and translate it into C code. The regular expressions were implemented using the PCRE library, which is a widely used library for pattern matching with regular expressions.

To output the resulting C code, the program simply printed it to the console using the printf function. This allowed the user to copy and paste the code into a file or an IDE for further editing or compilation.

Overall, the implementation made use of well-established libraries and tools to achieve its goals. The use of C and the BFD library provided a solid foundation for working with binary files and machine language, while the regular expressions and PCRE library allowed for efficient and accurate translation of assembly code to C.
