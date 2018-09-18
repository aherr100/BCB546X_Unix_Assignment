# BCB546X_Unix_Assignment

##Data Inspection 
###fang et al genotypes.txt
1. **File Size**
	1. `du -h fang_et_al_genotypes.txt`
		- File Size: 11M
2. **wc**
	1. `wc fang_et_al_genotypes.txt`
		- Number of Lines: 2783
		- Number of Words: 2744038
		- Number of Characters: 11051939 
2. **Head**
	1. `head fang_et_al_genotypes.txt`
		- Way to much printed to screen
	2. `head -n 2 fang_et_al_genotypes.txt` 
		- Still unreadable
	
2. **Tail**
	1. Same issues as with head
2. **Determine Number of Columns**
	1. `awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt`
		- Number of columns: 986
	2. `tail -n +4 fang_et_al_genotypes.txt | awk -F "\t" '{print NF; exit}'`
		- confirm column number
###snp_position.txt
1. **File Size**
	1. `du -h snp_position.txt`
		- File Size: 81K
2. **wc**
	1. `wc snp_position.txt`
		- Lines: 984
		- Words: 13198
		- Characters: 82763
2. **Head**
	1.	`column -t snp_position.txt | head -n 4`
		- Looks like SNP data
	
2. **Tail**
	1. `column -t snp_position.txt | tail -n 6`
		- confirmed snp data
	
2. **Determine Number of Columns**
	1. `awk -F "\t" '{print NF; exit}' snp_position.txt`
		- Number of Columns: 15

###Inspection Conclusion
####fang et al genotypes.txt
 - File Size: **11M**
 - Number of Columns: **986**
 - Number of Lines: **2783** 
####snp_position.txt
 - File Size: **81K**
 - Number of Column: **15**
 - Number of Lines: **984** 


##Data Processing
 
