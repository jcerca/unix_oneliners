# unix_oneliners
Scripts and one-liners to format data and do simple tasks required by fellow biologists. I hope this helps solving your problem

### Task 1 - splitting fasta files
#### Problem - Say you have a fasta with 120 sequences and you want to break it in "blocks" of 50 sequences (one file with sequence 1-50, another one with 51-100, and a third with 101-120).
### [Credit](https://www.biostars.org/p/153999/)
```
# input.fasta
# output.split.*.fa
awk 'BEGIN {n_seq=0;} /^>/ {if(n_seq%50==0){file=sprintf("output.split.%d.fa",n_seq);} print >> file; n_seq++; next;} { print >> file; }' < input.fasta

# How to to specify "blocks of 50"? Here: (n_seq%50==0)
# If you want blocks of 100. It would be (n_seq%100==0)
# The "%d" on the output file name will number the files. In the example above there will be three files: "output.split.0.fa" (sequences 1-50), "output.split..fa" (51-100), "output.split.2.fa" (101-120)
```



### Task 2 - cleaning names
#### Problem - Say you have a folder with >50 fastq files and you want to get all the names, but need to remove the "fastq.gz" from the end, and the "/location/from/the/beginning/". This  can be useful when you operate from different files.

###
```
ls /path/to/my/folder #These are my files.
/path/to/my/folder/T_waikamoi_Maui_WM_green_BioinformaticID_041.fastq.gz
/path/to/my/folder/T_waikamoi_Maui_WM_green_BioinformaticID_042.fastq.gz
/path/to/my/folder/T_waikamoi_Maui_WM_green_BioinformaticID_043.fastq.gz
/path/to/my/folder/T_waikamoi_Maui_WM_green_BioinformaticID_044.fastq.gz
/path/to/my/folder/T_waikamoi_Maui_WM_green_BioinformaticID_045.fastq.gz

for i in /path/to/my/folder/*.fastq.gz; do echo ${i#/path/to/my/folder/} | sed "s/.fastq.gz//g"

# Loop explained:
# for "i" in all "*.fastq.gz" files in the folder, do:
# the echo command will split the filename in the cardinal (#), which removes everything in the parameter "i" that starts with "/path/to/my/folder/" ### NOTE : If you wanted to remove first he "fastq.gz" in the end, you'd apply a "%" (# - beggining, % - end).
# sed just removes the fastq.gz
```


### Task 3 - deinterleaving a fasta
#### Problem - Many times, fasta files come with the sequence distributed in multiple sequences. It's often better to operate with fastas that are double-lined (i.e. one line with the header (starts with ">") and the second with the sequence.
[Source](https://www.ecseq.com/support/ngs-snippets/convert-interleaved-fasta-files-to-single-line)
###
```
awk '{if(NR==1) {print $0} else {if($0 ~ /^>/) {print "\n"$0} else {printf $0}}}' interleaved.fasta > new_fasta.fasta
```
