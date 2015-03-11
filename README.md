# Git Internals
 
 ## What makes a git repository?
 
 To create a git repository you just need the `.git` directory in your project, with a few essential subdirectories and files.
 
 Those subdirectories and files are:
 
 - HEAD - This file tells git where the HEAD of your repo currently is, it points to a ref
 - /refs - This directory conatains all the refs, or pointers, to commits, e.g. branches, tags etc...
 - /objects - This directory is the database of git, its the storage part of the key-value store on which git sits 

## The storage mechanism
 
 You can think of git as a wrapper over a content addressable key-value store, there are three contents types: blobs, trees and commits.
 
 Essentially, a blob is a file, a tree is a directory, which contains blobs or other trees and a commit is metadata for a tree.
 
 Git stores all of this in the objects directory. The process for doing so is like:
 
 - Add a header to the content consisting of the content type (blob/tree/commit) the content length and a null byte.
 - Use a SHA1 hashing algorithm on the header + content
 - Use zlib defalte compression on the hash
 
 The resulting compressed string is then stored in the objects directory, using the first 2 chars of the hash as the sub dir and the remaining as the file name.
 
 Git internals give use a command that wraps this process up for us: `git hash-object`. We also get the reverse of this command, which takes a hash and gives us the content which is `git cat-file`

