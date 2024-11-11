French wordlist by Tango  for Tails OS and the Tor Project. 
This document describes how 2 French wordlists (7776 and 8192 words) were created. 

A) Context: 
This initiative originates from Tails OS task 20014: 
https://gitlab.tails.boum.org/tails/tails/-/issues/20014

The intention is to offer offline capabilities to generate randoms lists of words with diceware software, in all tier-one languages. 
This was achieved in most languages by Jawlensky in December 2023, and Tango worked on the French one during summer 2024. 
There are about 100k distinct words in French, and a native speaker would use ~20k words.

Upstream maintainer of diceware, Ulif, has positively reviewed the first list of 7776 words (representing 5 rolls of 6 sided dices). Both Jawlensky and Ulif suggested to extend this list to 8192 words (2^13), making it faster for computations (lists that are 2^n long are better for machine-generated passwords). This was done by Tango in Oct 2024, as described below. 



B) Process for creating the 7776 words lookup table: 
The list is based on a 336k words list from Lorenbrichter called fr.txt and available here: 
https://github.com/lorenbrichter/Words/tree/master/Words

Lorenbrichter's list is licensed under the Creative Commons Zero v1.0 Universal, which is GPL compatible: 
https://www.gnu.org/licenses/license-list.html#CC0

Lorenbirchter's list was first filtered with the shell command below, then reviewed manually to remove rare words and other offensive words. 
cat fr_cut.txt | grep "...." | grep -v -e{".........","-","é","è","ê","ë","ï","î","ç","ô","ù","û","ü","à","â","ä","œ","æ","***s","merde","putain","chier","bordel","sodom","stupide","branle","foutre","encul","connard","connasse","salaud","salope","pute","nique","couille","abruti","queue","chatte","sex","viole","nichon","nibard","baise","anal","porno","fesse","gouine","salopard","youpin","youpine","burne","boche","bamboula","***aient","***iez","***eront"} | wc -l

Shell command above removes: 
. Words shorter than 4 characters
. Words longer than 8 characters
. hyphens "-"
. non-ascii
. all words containing "s" (intention was to remove plurals, but grep -v "***s" command was not correct, more on this below)
. the most common offensive words (taken from online lists)
. words ending with "-iez", "-aient", "-eront" (very likely to be conjugated forms).

Thousands of words have then been removed manually from the list to reach 7776 words. The focus was to remove anything that might be offensive and avoid rare words, so that the list can be used easily by native French speakers. 

Finally, the lookup table was generated: 
The list of 5 dices rolls (11111, 11112, 11114 ... 66666) was taken from an existing wordlist with the command:
$ cat wordlist_fr_5d.txt | cut -c 1-6 > num.txt

This rolls list and the words list were then assembled to create the lookup table (with space as separator):
$ paste -d '' num.txt 7776words.txt > wordlist_fr_7776.txt

and tested with
$ python3 rolldice wordlist_fr_7776.txt
This python code was found here:
https://github.com/mbelivo/diceware-wordlists-fr/blob/master/src/rolldice

Here are a few outputs:
. fabuler pince micron bible bidonner
. nubienne croyante amanite jazzmen nanti
. dada binaire taillage plaider paquet
. titre rentamer bulletin engage recru
. entropie activer adoptive raide terrible



C) Process for creating the 8192 words list: 
To generate this list, it was decided to add (to the list above) 8192-7776=416 words starting with "s", as they had all been filtered out by this wrong shell command:
grep -v "***s"

The correct shell/REGEX command to remove words ending with "s" is rather: 
grep -v "s\>" 

A list of 1665 words starting with "s" was generated using the command: 
cat fr.txt | grep "...." | grep "^s" | grep -v -e{"........","-","é","è","ê","ë","ï","î","ç","ô","ù","û","ü","à","â","ä","œ","æ","s\>"} > ext.txt

These words were then reviewed manually to remove anything that might be offensive and avoid rare words. 
Once this list was reduced to 416 words, it was added to the 7776 words list to obtain the desired 8192 words list. 

Here are a few outputs (with some "s"!):
. super nitrite fraudeur coffrage ouvrant
. viaduc planqua souda blondeur ignorer
. rocheux flanquer baluchon endormi orbite
. informa gazoline renie relation secret
. requiem rebond foula pergola pruneaux


D) Contacts: 
You can reach Tango about these lists at tango.tails@proton.me
and Ulif for diceware at uli@gnufix.de


E) License
Wordlists are distributed under License CC0 1.0 Universal
https://creativecommons.org/publicdomain/zero/1.0/legalcode



