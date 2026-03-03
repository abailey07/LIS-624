## 2026-02-15 -  Week 5 reflection 

**Goal:** To become familiar with the command `grep` and how to upload and search a BibTex file.

**Context:**  We are still building our foundational skills in the CLI, but this week added `grep` which felt similar to creating search strings.

**Steps:** I followed the directions in the lecture and in our readings.
	1. I practiced using `grep` with the given file operating-systems.csv.
	
	2. We explored several tack options to help our searching, including:
	
		1. `-i` to ignore case
		
		2. `-v` to search for lines that do not match
		
		3. ` ^ ` to indicate the start of a line (called regular expression or regex)
		
		4. ` $ ` to indicate the end of a line.
		
		5. `-c` to give a count of line containing a given thing
		
		6. ` | ` to use the Boolean OR search (alternate matching)
		
		7. `-w` to look for a whole word
		
		8. `-A NUM` and `-B NUM` to include a specificed number (NUM) of lines after or before the matching line
		
	3. I downloaded a search from Web of Science about music theory.  I downloaded a .bib of a thousand hits and uploaded the savedrecs.bib file to my VM.
	
	4. I explored this file using `grep` and several tack options.  I also explored how to use ` | ` to pipe outputs into the commands `sort` and `uniq`.
		

**Results:** It felt like I was creating search strings for research.  In making specifics in `grep`, we were able to search by keywords, get count, and do a little formatting to make things more readable in the VM.  These are things that an ILS does in its GUI for us, but in a CLI we have execute those commands ourselves. 

**Verification:** There were some errors along the way, particularly having failed partial commands.  In our discussion board, I learned that the `bash` will give another `>` as another opportunity to complete a mistyped commnad.  I can also use `ctrl-c` to cancel the command and get back to where I need to be.

**Notes:** I used `man grep` several times to look at some of the tack options.  It's still a bit overwhelming.