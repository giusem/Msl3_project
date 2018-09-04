#the peak table have "chr" in front of chromosome number
#have to add this to the annotation files

awk '{print"chr"$0}' GRCm37_release67_coding_TSS_pm1kb.bed > GRCm37_release67_coding_TSS_pm1kb_chr.bed 

awk '{print"chr"$0}' GRCm37_release67_coding_genes.bed > GRCm37_release67_coding_genes_chr.bed             

#intersect chip
#created new directory "intersect_chip_genes_TSS"

mkdir intersect_chip_TSS_genes_distal
cd intersect_chip_TSS_genes_distal

#from there ran:

for i in ../TomeksData_Peaks/*.bed; do base=$(basename $i); intersectBed -a $i -b ../GRCm37_release67_coding_TSS_pm1kb_chr.bed -wb > ${base%.bed}_TSS.tsv; done

for i in ../TomeksData_Peaks/*.bed; do base=$(basename $i); intersectBed -a $i -b ../GRCm37_release67_coding_genes_chr.bed -wb > ${base%.bed}_genes.tsv; done

