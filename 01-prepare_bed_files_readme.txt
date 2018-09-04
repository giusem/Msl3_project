
#took gencode genes from "/data/repository/organisms/GRCm37_ensembl/gencode/m1/genes.bed"
#took transcript 2 gene list from "/data/repository/organisms/GRCm37_ensembl/gencode/m1/transcript2gene.txt"


cut -f 1,2,3,4,6 /data/repository/organisms/GRCm37_ensembl/gencode/m1/genes.bed > temp.bed

awk '{if($1<20 || $1=="X" || $1=="Y")print$0}' temp.bed > main_temp.bed
#this excluded only the mitochondria chromosome

#TSS plus minus 1kb
awk '{if($5=="+")print$1,$2-1000,$2+1000,$4; else print$1,$3-1000,$3+1000,$4}' main_temp.bed | tr [:blank:] \\\t > TSStemp.bed

####

#converted transcript ids to human readable GENE names using the transcript_2_gene_converter_example.R script
#output: "GRCm37_release67_TSS_pm1kb.bed"
#update: will not use the human readable gene names but the mouse gene id instead
#so I modified the R script a bit

####


#also created a bed file of genes from main_temp.bed

cut -f 1,2,3,4 main_temp.bed > genes_temp.bed

#converted transcript ids to human readable GENE names using the transcript_2_gene_converter_example.R script
#output: "GRCm37_release67_genes.bed"
#update: as before, I needed gene ids instead so used the modified R script
#update: see below, this file was wrong and needed to be redone with genes+1kb upstream



####

#update: 2018-03-07
#realized that the distal and TSS peaks share some - because I used for TSS +/- 1kb but for distal the genes as reference
#so I need to use a genes +1kb upstream to avoid an overlap

#gene plus 1kb 
awk '{if($5=="+")print$1,$2-1000,$3,$4; else print$1,$2,$3+1000,$4}' main_temp.bed | tr [:blank:] \\\t > genes_temp2.bed

####

#converted transcript ids to human readable GENE names using the transcript_2_gene_converter_example.R script
#output: "GRCm37_release67_genes_plus1KBupstream.bed"
#update: will not use the human readable gene names but the mouse gene id instead
#so I modified the R script a bit



####

#cleanup
rm temp.bed
rm main_temp.bed
rm genes_temp.bed
rm genes_temp2.bed

