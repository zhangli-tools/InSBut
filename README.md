# BUSTIN
BUSTIN (Bulk and Single-cell transcriptome data integration) is an R package, which identifies cell subpopulations that primarily express the genes of your interest, e.g. the drug resistant genes, by integrating bulk and single-cell transcriptome data.

# Installation
BiocManager::install(c("Seurat","DESeq2","limma")) <br>
devtools::install_github("zhangli-tools/BUSTIN")<br>
# Usage
library(Seurat)<br>
library(BUSTIN)<br>
## Idnetify the drug resistant genes from bulk data <br>
resistant.genes=run.limma(bulk.data, pdata, resistant=T, padj=0.05, log2fc=0.5)
## if you use the bulk RNA-seq data <br>
resistant.genes=run.DESeq(bulk.data, pdata, resistant=T, padj=0.05, log2fc=0.5)
## Build a hierachical clustering tree for resistant genes based on scRNA-seq data <br>
hc.resistant.genes=hclust.geneset(seurat.obj = seurat.obj,maxK = 10,geneset = resistant.genes) <br>
## Predict the phenotype-associated cells<br>
BUSTIN.out=predict.PAC(seurat.obj = seurat.obj,k.out.list = list(hc.resistant.genes),minModuleSize = 15) <br>
## Identify the cell subpopulations associated with phenotype (drug resistance).<br>
cell.type=ORA.celltype(BUSTIN.out$seurat.obj,features = grep("m\[0-9\]\+$", colnames(BUSTIN.out$seurat.obj\@meta.data), value=T),group.by = "label") <br>
