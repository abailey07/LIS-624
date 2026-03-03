# LIS-624
### All things related to LIS 624 in Spring 2026 semester

I am working to learning some of the skills a systems librarian needs to do their job effectively.  This repository will be used with a virtual machine running Linux where I will learn how to use a command line to create and execute programs related to systems librarianship.
This is all new to me.  I **struggled** with using DOS for games in the 90s, so this is a stretch for me.  I will be documenting my process.

## 2026-02-03 - Setting things up (updated 2026-03-03)
* Goal: Connect this repository with my VM
* Context: This is my work for Week 4 of classes.
* Steps: I am following the steps laid out in our textbook reading and discussion post.
    1. Selected Tilde as text editor and installed using the following command
        ```
        sudo apt install tilde
        ```
    2. I chose to not make any changes to the Bash shell colors.
    3. I created a repository for LIS-624 in my already established Git.
    4. We learned some basics of Markdown and how to use this in our selected text editor.
    5. I connected my VM to my Git, established connection to the main branch, and set Tilde as my main editor with these commands:
        ```
        git config --global user.name "abailey07"
        git config --global user.email amanda.bailey@uky.edu
        git config --global init.defaultBranch main
        git config --global core.editor "tilde"
    6. We secured the connection using SSH key:
        ```
        ssh-keygen -t ed25519 -C "amanda.bailey@uky.edu"
        cat ~/.ssh/id_ed25519.pub
        ```

        I copied it and then in GitHub I added it in the repo settings.  Back in my VM, I configured the sign-in"

        ```
        git config --global gpg.format ssh
        git config --global user.signingkey $HOME/.ssh/id_ed25519.pub
        git config --global commit.gpgsign true
        ```
    7. I cloned my repo in my VM:
        ```
        git clone git@github.com:abailey07/LIS-624.git
        ```
    8. I learned how to push and pull (as well as stage and commit) files from my VM to my repo.
        ```
        git add name-of-file.md
        git commit -m "note about file"
        git push origin main
        ```
* Results: All tasks were completed with no issue.
* Verification: I committed and pushed this README from my VM.  I have also continued to make edits using this method.  My VM also shows my git status is connected.
* Notes: Make sure you put spaces after markups to make sure they take effect.

## Added in VM

I've added something new while in the VM.  

### Update on 2026-02-04

I'm trying to remember how to do things without looking them up first.  I don't remember how to add comments on the update or how to push, so I will need to rewatch that part of the video.

However, I was able to apply the new updates to my VM and successfully open this text document in tilde to type this.  I consider that a win!