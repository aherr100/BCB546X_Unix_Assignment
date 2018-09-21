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
		- desired columns are 1,3,4
	
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
 - This is genotype data 
####snp_position.txt
 - File Size: **81K**
 - Number of Column: **15**
 - Number of Lines: **984** 
 - This is position data


##Data Processing
###Maize
1.  Create Maize Genotype file
	- `head -1 fang_et_al_genotypes.txt > maize_geno.txt`
	- `grep 'ZMMMR' fang_et_al_genotypes.txt >> maize_geno.txt`
	- `grep 'ZMMIL' fang_et_al_genotypes.txt >> maize_geno.txt`
	- `grep 'ZMMLR' fang_et_al_genotypes.txt >> maize_geno.txt`
2. Transpose Data
	- `awk -f transpose.awk maize_geno.txt > maize_geno_trans.txt`
3. Remove unwanted header lines
	- `sed -i '/JG_OTU/d' maize_geno_trans.txt`
	- `sed -i '/Group/d' maize_geno_trans.txt`
4. Change Sample_ID to SNP_ID
	- `sed -i 's/Sample_ID/SNP_ID/g' maize_geno_trans.txt`
3. Isolate Head, Sort Body, Merge back (Genotype)
	- `tail -n +2 maize_geno_trans.txt > maize_geno_body.txt`
	- `head -1 maize_geno_trans.txt > maize_final.txt`
	- `sort -k1,1 maize_geno_body.txt >> maize_final.txt`
2. Isolate Head, Sort Body, Merge back (Position) 
	- `head -n 1 snp_position.txt > snp_final.txt`
	- `tail -n +2 snp_position.txt | sort -k1,1 > snp_final.txt`
3. Join Files 
	- `join -1 1 -2 1 -t $'\t' snp_final.txt maize_final.txt > joined_maize.txt`
8. Create file w/o multiple positions 
	- `grep -i -v 'multiple' joined_maize.txt > joined_maize_womultiple.txt`
9. Create Files for 10 chr in Increasing Order and ? as Blanks
	- `for i in {1..10}; do (head -1 joined_maize_womultiple.txt; awk '$2 ~/^'$i'$/ { print $0 }' joined_maize_womultiple.txt | sort -k3,3n) > chr"$i"_in_maize.txt; done`
9. Create Files for 10 chr in Decreasing Order and - as Blanks
	- `for i in {1..10}; do ( head -1 joined_maize_womultiple.txt; awk '$2 ~/^'$i'$/ { print $0 }' joined_maize_womultiple.txt | sort -k3,3nr | sed s/?/-/g) > chr"$i"_de_maize.txt; done`
10. Isolate SNPs with Unknown Position
	- `awk '$3 ~/unknown|Position/ { print $0 }' joined_maize.txt > chr_unknown_maize.txt`
11. Isolate SNPs with Multiple Positions
	- `awk '$3 ~/multiple|Position/ { print $0 }' joined_maize.txt > chr_multiple_maize.txt`
###Teosinte
1. Repeat all steps except select ZMPBA|ZMPIL|ZMPJA instead of ZMMIL|ZMMLR|ZMMMR
