# Neovim keyboard shortcuts

#### Normal mode shortcut keys
 **gq**  Formats highlighted text, for example wraps text to resetricted length
 after it has been copied into Neovim.  <!--TODO:  Need to build a better
 understanding of why this and where this is setp.  I potentially have this
 mapped somewhere-->
This does not appear in Whichkey

#### Insert mode keyboard shortcuts

**Ctrl-h** functions like the backspace key.  Potentially could also use the base
/ palm of right hand to hit ctrl and right hand to also hit h


**U** Undo all changes in the current line - unlike small u that only undoes the
current change.

#### Ex-commands

##### Substittion
 s/thee/the/
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Replaces first
 match in current line <br> 
 s/thee/the/g
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Replaces all matches in current
 line <br>
 %s/thee/the &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Replaces
 first occurence in each line of entire file <br>
 %s/thee/the/g
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Replaces all occurances in entire file <br>
 %s/thee/the/gc &nbsp;&nbsp;&nbsp;Replaces ll occurences in etire file with
 confirmation

