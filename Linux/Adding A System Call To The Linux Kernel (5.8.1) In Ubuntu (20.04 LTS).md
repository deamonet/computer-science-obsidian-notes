[Skip to content](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#main-content)

[![DEV Community](media/DEV_Community.png)](https://dev.to/)

[Log in](https://dev.to/enter) [Create account](https://dev.to/enter?state=new-user)

![Cover image for Adding A System Call To The Linux Kernel (5.8.1) In Ubuntu (20.04 LTS)](media/Cover_image_for_Adding_A_System_Call_To_The_Linux_Kernel_(5.8.1)_In_Ubuntu_(20.04_LTS).jpg)

[![Jihan Jasper Al-rashid](media/Jihan_Jasper_Al-rashid.jpg)](https://dev.to/jasper)

[Jihan Jasper Al-rashid](https://dev.to/jasper)

Posted on Aug 13, 2020 • Updated on Aug 14, 2020

 ![](https://dev.to/assets/sparkle-heart-5f9bee3767e18deb1bb725290cb151c25234768a0e9a2bd39370c382d02920cf.svg) 11![](https://dev.to/assets/multi-unicorn-b44d6f8c23cdd00964192bedc38af3e82463978aa611b4365bd33a0f1f4f3e97.svg) 2  

# Adding A System Call To The Linux Kernel (5.8.1) In Ubuntu (20.04 LTS)

[#beginners](https://dev.to/t/beginners) [#tutorial](https://dev.to/t/tutorial) [#linux](https://dev.to/t/linux) [#kernel](https://dev.to/t/kernel)

**In this guide, you will learn how to add a simple system call to the Linux kernel. Check the help sections on the table of contents if you need help with text editors.**

[Section 1 - Preparation](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#section-1)  
[Section 2 - Creation](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#section-2)  
[Section 3 - Installation](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#section-3)  
[Section 4 - Result](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#section-4)  
[Help - Text Editors](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#help-1)  
[Help - `nano`](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#help-2)

# [](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#section-1-preparation)**Section 1** - Preparation

_In this section, you will download all necessary tools to add a basic system call to the Linux kernel and run it. This is the only part of the entire process where network connectivity is necessary._

**1.1** - Fully update your operating system.  

```
sudo apt update && sudo apt upgrade -y
```

**1.2** - Download and install the essential packages to compile kernels.  

```
sudo apt install build-essential libncurses-dev libssl-dev libelf-dev bison flex -y
```

If would rather use `vim` or any other text editor instead of `nano`, below is an example of how you install it.  

```
sudo apt install vim -y
```

**1.3** - Clean up your installed packages.  

```
sudo apt clean && sudo apt autoremove -y
```

**1.4** - Download the source code of the latest stable version of the Linux kernel (_which is 5.8.1 as of 12 August 2020_) to your home folder.  

```
wget -P ~/ https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.8.1.tar.xz
```

If you have downloaded a newer version of the Linux kernel, refer to [this documentation](https://www.kernel.org/doc/html/latest/process/adding-syscalls.html) to learn about any relevant change made to system calls.

**1.5** - Unpack the tarball you just downloaded to your home folder.  

```
tar -xvf ~/linux-5.8.1.tar.xz -C ~/
```

**1.6** - Reboot your computer.

# [](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#section-2-creation)**Section 2** - Creation

_In this section, you will write a basic system call in C and integrate it into the new kernel._

**2.1** - Check the version of your current kernel.  

```
uname -r
```

As of 12 August 2020, it should display the following.

`5.4.0-42-generic`

In section 4, it should be different.

**2.2** - Change your working directory to the root directory of the recently unpacked source code.  

```
cd ~/linux-5.8.1/
```

**2.3** - Create the home directory of your system call.

Decide a name for your system call, and keep it consistent from this point onwards. I have chosen `identity`.  

```
mkdir identity
```

**2.4** - Create a C file for your system call.

Create the C file with the following command.  

```
nano identity/identity.c
```

Write the following code in it.  

```
#include <linux/kernel.h>
#include <linux/syscalls.h>

SYSCALL_DEFINE0(identity)

{
    printk("I am Jihan Jasper Al-rashid.\n");
    return 0;
}
```

You can write anything you like here.

Save it and exit the text editor.

**2.5** - Create a Makefile for your system call.

Create the Makefile with the following command.  

```
nano identity/Makefile
```

Write the following code in it.  

```
obj-y := identity.o
```

Save it and exit the text editor.

**2.6** - Add the home directory of your system call to the main Makefile of the kernel.

Open the Makefile with the following command.  

```
nano Makefile
```

Search for `core-y`. In the second result, you will see a series of directories.

`kernel/ certs/ mm/ fs/ ipc/ security/ crypto/ block/`

In the fresh source code of Linux 5.8.1 kernel, it should be in line 1073.

Add the home directory of your system call at the end like the following.  

```
kernel/ certs/ mm/ fs/ ipc/ security/ crypto/ block/ identity/
```

Save it and exit the editor.

**2.7** - Add a corresponding function prototype for your system call to the header file of system calls.

Open the header file with the following command.  

```
nano include/linux/syscalls.h
```

Navigate to the bottom of it and write the following code just above `#endif`.  

```
asmlinkage long sys_identity(void);
```

Save it and exit the editor.

**2.8** - Add your system call to the kernel's system call table.

Open the table with the following command.  

```
nano arch/x86/entry/syscalls/syscall_64.tbl
```

Navigate to the bottom of it. You will find a series of x32 system calls. Scroll to the section above it. This is the section of your interest. Add the following code at the end of this section respecting the chronology of the row as well as the format of the column. Use Tab for space.  

```
440     common  identity                sys_identity
```

In the fresh source code of Linux 5.8.1 kernel, the number for your system call should be 440.

Save it and exit the editor.

# [](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#section-3-installation)**Section 3** - Installation

_In this section, you will install the new kernel and prepare your operating system to boot into it._

**3.1** - Configure the kernel.

Make sure the window of your terminal is maximized.

Open the configuration window with the following command.  

```
make menuconfig
```

Use **Tab** to move between options. Make no changes to keep it in default settings.

Save and exit.

**3.2** - Find out how many logical cores you have.  

```
nproc
```

The following few commands require a long time to be executed. Parallel processing will greatly speed them up. For me, it is `12`. Therefore, I will put 12 after `-j` in the following commands.

**3.3** - Compile the kernel's source code.  

```
make -j12
```

**3.4** - Prepare the installer of the kernel.  

```
sudo make modules_install -j12
```

**3.5** - Install the kernel.  

```
sudo make install -j12
```

**3.6** - Update the bootloader of the operating system with the new kernel.  

```
sudo update-grub
```

**3.7** - Reboot your computer.

# [](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#section-4-result)**Section 4** - Result

_In this section, you will write a C program to check whether your system call works or not. After that, you will see your system call in action._

**4.1** - Check the version of your current kernel.  

```
uname -r
```

It should display the following.

`5.8.1`

**4.2** - Change your working directory to your home directory.  

```
cd ~
```

**4.3** - Create a C file to generate a report of the success or failure of your system call.

Create the C file with the following command.  

```
nano report.c
```

Write the following code in it.  

```
#include <linux/kernel.h>
#include <sys/syscall.h>
#include <stdio.h>
#include <unistd.h>
#include <string.h>
#include <errno.h>

#define __NR_identity 440

long identity_syscall(void)
{
    return syscall(__NR_identity);
}

int main(int argc, char *argv[])
{
    long activity;
    activity = identity_syscall();

    if(activity < 0)
    {
        perror("Sorry, Jasper. Your system call appears to have failed.");
    }

    else
    {
        printf("Congratulations, Jasper! Your system call is functional. Run the command dmesg in the terminal and find out!\n");
    }

    return 0;
}
```

You can customize the messages for failure and success anyhow you like.

Save it and exit the editor.

**4.4** - Compile the C file you just created.  

```
gcc -o report report.c
```

**4.5** - Run the C file you just compiled.  

```
./report
```

If it displays the following, everything is working as intended.

`Congratulations, Jasper! Your system call is functional. Run the command dmesg in the terminal and find out!`

**4.6** - Check the last line of the `dmesg` output.  

```
dmesg
```

At the bottom, you should now see the following.

`I am Jihan Jasper Al-rashid.`

# [](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#congratulations-you-have-successfully-added-a-system-call-to-the-linux-kernel)**Congratulations! You have successfully added a system call to the Linux kernel!**

# [](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#help-text-editors)**Help - Text Editors**

This guide has frequent instances of text editing. You can use whatever gives you comfort. For the purpose of this guide, I have used `nano`. If you would rather use `vim` or any other text editor, simply replace `nano` with either `vim` or any other text editor in all of the commands. If you would rather use a GUI-based text editor, the process is the same. `gedit`, GNOME's default GUI-based text editor, comes bundled with Ubuntu 20.04 LTS.

# [](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#help-basic-controls-for-raw-nano-endraw-)**Help - Basic Controls For `nano`**

**Ctrl + O** brings up the interface to save a document. Pressing **Enter** afterwards will complete the process.

**Ctrl + W** brings up the interface to search for anything in a document. You can write anything in here and press **Enter** to jump to it. The result will be highlighted.

**Alt + W** takes you to the next instance, if there is one, after you searched anything once.

**Ctrl + C** cancels any interface.

**Alt + C** turns on the display which shows you the line number based on the position of your cursor.

**Ctrl + X** exits the text editor.

## Top comments (3)

![pic](media/pic.png)

 

[![peterlitszo profile image](media/peterlitszo_profile_image.png)](https://dev.to/peterlitszo)

• [May 19 '22](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#comment-1ogmn)

What a great article it is! If you know how to add a system call by a module, it will be better!

[](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8/comments/new/1ogmn)

[Reply](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8/comments/new/1ogmn)

 

[![templarassvn profile image](media/templarassvn_profile_image.png)](https://dev.to/templarassvn)

• [Jan 2 '21 • Edited on Jan 2](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#comment-19pik)

Tks a lot! I have a question. Is there anything else to do if I want to add 2 syscalls at once?

[](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8/comments/new/19pik)

[Reply](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8/comments/new/19pik)

 

[![ktnguy23 profile image](media/ktnguy23_profile_image.png)](https://dev.to/ktnguy23)

• [Sep 2 '21](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#comment-1hkk3)

arch/x86/entry/syscall_64.o:(.rodata+0xdf8): undefined reference to

I got this error with Kernel 5.13. Does anyone know where should I modify in this tutorial ? Many thanks

[](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8/comments/new/1hkk3)

[Reply](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8/comments/new/1hkk3)

[Code of Conduct](https://dev.to/code-of-conduct) • [Report abuse](https://dev.to/report-abuse)

DEV Community

## [](https://dev.to/jasper/adding-a-system-call-to-the-linux-kernel-5-8-1-in-ubuntu-20-04-lts-2ga8#50-cli-tools-you-cant-live-without)[50 CLI Tools You Can't Live Without](https://dev.to/lissy93/cli-tools-you-cant-live-without-57f6)

[![](media/zoxide.gif)](https://dev.to/lissy93/cli-tools-you-cant-live-without-57f6)

The top 50 must-have CLI tools, including some scripts to help you automate the installation and updating of these tools on various systems/distros.

## Read next

[

![jobizil profile image](media/jobizil_profile_image.png)

### Day 9: Python Built In Modules

Ugbem Job - Feb 22





](https://dev.to/jobizil/day-9-python-built-in-modules-4i8g)[

![efkumah profile image](media/efkumah_profile_image.jpg)

### An easy guide to understanding databases

Emmanuel Fordjour Kumah - Feb 21





](https://dev.to/efkumah/an-easy-guide-to-understanding-databases-2hcf)[

![iayeshasahar profile image](media/iayeshasahar_profile_image.jpg)

### Debugging JavaScript Like a Pro: Tools and Techniques for Finding and Fixing Bugs

Ayesha Sahar - Feb 25





](https://dev.to/iayeshasahar/debugging-javascript-like-a-pro-tools-and-techniques-for-finding-and-fixing-bugs-2lf5)[

![islomali99 profile image](media/islomali99_profile_image.jpg)

### C++ Sintaksis

islomAli99 - Feb 21





](https://dev.to/islomali99/c-sintaksis-3gge)

 [![](media/820ced9f-28a8-43b3-a606-2b690c983588.jpeg.jpg)Jihan Jasper Al-rashid](https://dev.to/jasper)

-   Location
    
    Dhaka, Bangladesh
    
-   Joined
    
    Aug 11, 2020
    

### Trending on [DEV Community](https://dev.to)

[![Varshith V Hegde profile image](media/Varshith_V_Hegde_profile_image.jpg)

Achieving 1K Followers on dev.to: My Journey to Success

#beginners #productivity #webdev #javascript



](https://dev.to/varshithvhegde/achieving-1k-followers-on-devto-my-journey-to-success-201n)[![Ben Halpern profile image](media/Ben_Halpern_profile_image.jpg)

New Programmers, What Challenges Are You Facing?

#discuss #beginners #codenewbie



](https://dev.to/codenewbieteam/new-programmers-what-challenges-are-you-facing-45k9)[![Ben Halpern profile image](media/Ben_Halpern_profile_image.jpg)

New Programmers, What Challenges Are You Facing?

#discuss #beginners #codenewbie



](https://dev.to/codenewbieteam/new-programmers-what-challenges-are-you-facing-45k9)

DEV Community

![Image description](media/Image_description.jpg)  
[Create An Account on DEV](https://dev.to/enter?state=new-user).

[DEV Community](https://dev.to/) — A constructive and inclusive social network for software developers. With you every step of your journey.

-   [Home](https://dev.to/)
-   [Listings](https://dev.to/listings)
-   [Podcasts](https://dev.to/pod)
-   [Videos](https://dev.to/videos)
-   [Tags](https://dev.to/tags)
-   [FAQ](https://dev.to/faq)
-   [Forem Shop](https://shop.forem.com)
-   [Sponsors](https://dev.to/sponsorships)
-   [About](https://dev.to/about)
-   [Contact](https://dev.to/contact)
-   [Guides](https://dev.to/guides)
-   [Software comparisons](https://dev.to/software-comparisons)

-   [Code of Conduct](https://dev.to/code-of-conduct)
-   [Privacy Policy](https://dev.to/privacy)
-   [Terms of use](https://dev.to/terms)

Built on [Forem](https://www.forem.com) — the [open source](https://dev.to/t/opensource) software that powers [DEV](https://dev.to) and other inclusive communities.

Made with love and [Ruby on Rails](https://dev.to/t/rails). DEV Community © 2016 - 2023.