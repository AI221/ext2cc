# ext2cc

An "Ext2-like" filesystem for CC. Primarily for having a single filesystem across multiple floppy drives, or for OSes.

# How it works

## Abstraction layers

<image src="i.imgur.com/WRfZpOdg.png"></image>

## Inodes

Ext2CC Ext2 CC uses inodes(https://en.wikipedia.org/wiki/Inode) to store metadata, what blocks the file is on, and what directorieS, yes, that S was no accident. One file can be in multiple directories. Do note that the file's contents themselves are NOT stored in the inode. Those are stored in blocks.

## Blocks

Blocks in Ext2CC share little in common with real-world blocks. First of all, each block is actually just a file on CC's actual file system(same for the inode), second of all, they are dynamically spaced, and third of all, 1 partition can have blocks on multiple drives. They are to real life blocks as lua tables are to C arrays. 



## Directories
Directories are rather simple. They're a table that look like this:

    {

        [".yournotsosecretfile"] = 3,
    
        ["folder"] = 4,
    
        ["Folder"] = 5,
    
    }

(yes, Ext2CC is case sensative)

So, what that is is the file name associated with the inode number. So when I go to look for "/folder/file", Ext2CC starts off looking in the root file(which is always at inode #2), finds that "folder"'s inode is 4, looks in folder, and then finds that file's inode number is 6. Note that the file contents are NOT stored in the inode, they are in the blocks. I simplified a bit.

## Clarification/misc

The root folder is at_inode_2, *not* block 2. 

File names are_not_stored in the inode.

To find a file, you must always start at the root folder and find the next folder leading to the file, till you're at the file

Folders are files. 

Inode & folders are saved as a serialized table

# License

See the file called LICENSE at the root of this git.
