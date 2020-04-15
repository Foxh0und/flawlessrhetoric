---
layout: post
title: "Using libpst to convert PST to MBOX, and understanding Thunderbird's folder structure.md"
description: "Using open source tools to import mail from Outlook to Thunderbird"
tag: Computers
---
## Introduction
I have recently moved my mail hosting from Office 365 to [Mailbox.org](mailbox.org), and needed to move my mail outside of Outlook. 

Once I had the PST exported, I needed to import it into Thunderbird on my Linux machine. There are a plethora of tools, mostly Windows based, and all require payment for their full usage, and even then, often don't yield the required result.

## Understanding MBOX
MBOX is an open source file format that represents a mailbox containing one or more mails, and was first introduced with the Fifth Edition of Unix in 1974 [1].

In simplified terms, when picturing your mail client, a single mbox file is a folder with a number of messages inside it.

## Thunderbird's Folder Structure
Thunderbird uses a folder structure that can be somewhat challenging to understand, and isn't clearly laid out.


Under the hood in the file system, each mbox file represents a folder. However, each folder if is also a directory with it's own sub folders, must be a directory object with the `.sbd` suffix.
If the folder is just a subdirectory with no mail, it's parent directory must contain a plain file name with the same name as the directory( without the .sbd)


For example, consider the following directory


```
+-- Archive.sbd
|   +-- School.mbox
|   +-- Work.mbox
|   +-- Bills.sbd
    |   +-- Cars.sbd
    |   +-- House.mbox
    |   +-- Cars.mbox
    +-- Bills
```
<br>
The top level folder is Archive, and it has three children, School, Work, and Bills.
School and work have no children of their own, but have their own mail. However, Bills has sub directories, so it has a directory with the .sbd suffix. It doesn't have it's own mail however, so it has the empty file named Bills.
Inside Bills, there is a mailbox named House, and a Cars mailbox, that has not only mail, but also may contain sub children, hence why there is the Cars.sbd directory.
<br>
It's not entirely straight forward, but once you see some examples, it's pretty easy to understand. [This forum post](http://colby.id.au/importing-pst-files-into-thunderbird-using-libpst/) from 2011 helped me understand it, as well as how to modify some commands to import PSTs to Thunderbird.


## The Process

### Libpst Installation

#### Ubuntu, Debian
`sudo apt-get install -y libpst-dev`

#### Fedore, RHEL, CentOS
`sudo yum install -y libpst`

### Preparation
First, you need to convert the pst, maintaining the folder structure for Thunderbird, using libpst's readpst functionality.

`readpst -u <pstname>.pst`

Now, copy it into a more malleably named directory.

`mkdir out`

`mv Outlook\ Data\ File out`

Add the .sbd extension to the directories.


`find out -type d | tac | grep -v '^out$' | xargs -d '\n' -I{} mv {} {}.sbd`


Remove the .mbox extension from the files, and move them into their parent directory.


`find out -name mbox -type f | xargs -d '\n' -I{} echo '"{}" "{}"' | sed -e 's/\.sbd\/mbox"$/"/' | xargs -L 1 mv`


Now, remove any empty directories if they exist. Don't worry if this fails, it just means you don't have any.


`find out -empty -type d | xargs -d '\n' rmdir`


Finally, we need to add the files that signify no mailbox, but a subdirectory.


`find out -type d | egrep *.sbd | sed 's/.\{4\}$//' | xargs -d '\n' touch`



### Importing into Thunderbird
Thunderbird's data is usually located under `~/.thunderbird/<id>`, and you'll need to manually check what your id is.


I prefer to add my folders to my Local Computer before dragging it into my IMAP account, but you can modify the final command to copy it into the IMAP Folders


`cp -r out/* ~/.thunderbird/<id>/Mail/Local\ Folders/`


## References
[1]"Mbox", En.wikipedia.org, 2020. [Online]. Available: https://en.wikipedia.org/wiki/Mbox. [Accessed: 15- Apr- 2020]
