# This pipeline used to call variants, no biological duplication. Two main parts: generating BAM files, calling variants
# Please strictly follow my steps and samples naming
# GATK call variant
# 1. Command-line argument is work_dir

1. Parameter explanation

   work_dir=$1             ($1 that means your command-line argument, my work_dir is: /home/ruizhi/data/wgs_pipeline)

   reference=/home/ruizhi/data/wgs_pipeline/reference/genome                     (hg38 is stored here)

   GATK_bundle=/home/ruizhi/data/wgs_pipeline/resources/gatk/bundle/hg38         (GATK bundle is stored here)

2. your fasta files are stored at ${work_dir}/fasta_file, like /home/ruizhi/data/wgs_pipeline//fasta_file

   And please put all of your fasta files to ${work_dir}/fasta_file
   
   We will use bwa to map reads to hg38.

3. GATK bundle is stored at ${GATK_bundle}, like /home/ruizhi/data/wgs_pipeline/resources/gatk/bundle/hg38
   
   And please download all of your GATK bundle to ${GATK_bundle}
   
   GATK bundle include all resource that we need for gatk variant calling.
   
   You can find instruction for how to download those data. Please follow my instruction to download the same GATK bundle, 
   
   then you don't need to build any index for those gatk bundle.

4. This script just used to analyze non-biological samples. Your fasta files are pair-end sequecing.

   The format of sample names must be: SAMPLE-ID_*.1.fq.gz      SAMPLE-ID_*.2.fq.gz
   
   like KPGP-00246_L001_R1.fq.gz       KPGP-00246_L001_R2.fq.gz
   
        KPGP-00246_*1.fq.gz             KPGP-00246_*2.fq.gz
        
   Please strictly follow my naming rule!!!

# 2. Preparation

1. Please USING conda to intall softwares:

   conda install trim_galore
   
   conda install bwa
   
   conda install samtools
 
   conda install gatk/4.1.2.0 (if you can't conda gatk/4.1.2.0, please intall by source, and add to enciroment varible)

2. Please download human reference genome hg38 to reference folder, like /home/ruizhi/data/wgs_pipeline/reference/genome

   cd ${reference} 
  
   wget http://hgdownload.cse.ucsc.edu/goldenPath.hg38/bigZips/hg38.fa.gz

3. please download GATK bundle and stored at ${GATK_bundle}. (recommend use FileZilla)

   GATK bundle can be downloaded from ftp-site (LEAVE THE PASSWORD BLANK):

   ftp.broadinstitute.org/bundle

   username: gsapubftp-anonymous

   password:
   
# 3. After installed sofwares and download resources, you can RUN script

1. Firstly, run preparation.sh

   ./preparation.sh /work_dir
   
   mine is:
   
   ./preparation.sh /home/ruizhi/data/wgs_pipeline
   
   
2. Then, qc and mapping, get bam files

   ./fastq_to_bam.sh /work_dir
   
    mine is:
    
   ./fastq_to_bam.sh /home/ruizhi/data/wgs_pipeline

3. Last, call variant
   
   ./GATK_variant_call.sh /work_dir
   
   mine is:
   
   ./fastq_to_bam.sh /home/ruizhi/data/wgs_pipeline

# 4. The result of high-quality VCFs are stored at ${work_dir}/result/gatk/VCF

# Platypus call variant
# assume you already get sorted.bam files by fastq_to_bam.sh

1. Please download Platypus from https://github.com/RahmanTeamDevelopment/Platypus

   install Platypus instruction, before install Platupus, you have to first install Virttualenv and Python (version 2.7.X is currently supported)
   
   conda install Virttualenv
   
   cd /Platypus

   ./install.sh

   Be careful! when you install platypus, your python will be downgraded to 2.7 version!!!

2. command-line arguments

   work_dir=$1

   platypus=$2

   (my work_dir=/home/ruizhi/data/wgs_pipeline)

   (platypus=$2 This is your package platypus installed location, my platypus=/home/ruizhi/data/biosoft/Platypus-master/env/bin/platypus)

3. bam files are stored at ${work_dir}/result/bwa

4. RUN script
   
   ./platypus.sh /work_dir/ /your_path_to_installed_platypus/
   
   like ./platypus.sh /home/ruizhi/data/wgs_pipeline /home/ruizhi/data/biosoft/Platypus-master/env/bin/platypus
   
5. result of platypus will be stored at ${work_dir}/result/platypus


