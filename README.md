# unix_oneliners
Scripts and one-liners to format data and do simple tasks required by fellow biologists. I hope this helps solving your problem

###
#Task 1 - splitting fasta files
Say you have a fasta with 120 lines and you want to break it in "blocks" of 50 sequences (one file with sequence 1-50, another one with 51-100, and a third with 101-120)
```
awk 'BEGIN {n_seq=0;} /^>/ {if(n_seq%50==0){file=sprintf("output.file.name.%d.fa",n_seq);} print >> file; n_seq++; next;} { print >> file; }' < input.fasta

#How to to specify "blocks of 50"? Here: (n_seq%50==0)
# If you want blocks of 100. It would be (n_seq%100==0)
# The "%d" on the output file name will number the files. In the example above there will be three files: "0" (sequences 1-50), "1" (51-100), "2" (101-120)
```
