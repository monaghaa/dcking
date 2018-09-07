BootStrap: docker
From: ubuntu:16.04

##################################
#notes from andy:

# most bioinformatics tools required by Nishimura lab are available
# in bioconda. I installed the latest version of each tool
#   but specifc versions can be installed if needed: 
#   for specific version of a bioconda package, e.g.: bwa==0.7.17
#
# Exceptions: 
# 1. DESeq2: I installed this directly in R bioconductor
#    (there actually is a bioconda 'bioconductor-deseq2' package
#    but installing it was problematic)
# 2. javaGenomicsToolkit: Installed from source code (required java and ant)
# 3. ucsc apps: There actually are some apps available in bioconda
#               (ucsc-liftover and ucsc-blat)
#               but I wasn't sure which were needed so I installed
#               from source so that everyting is available
#               it isn't clear if the tools requiring mysql will 
#               work.  If these tools are needed, we could install
#               mysql-server 

##################################

%post

    # install some system deps
    apt-get -y update
    apt-get -y install locales curl bzip2 less unzip git wget vim ant default-jdk
    # this is a X11 dep 
    apt-get -y install libxext6
    # tools to open PDF and HTML files
    apt-get -y install firefox xpdf
    # some extra devel libs
    apt-get -y install zlib1g-dev libssl-dev libpng-dev uuid-dev 
    # other
    locale-gen en_US.UTF-8
    apt-get clean
   
    # download and install miniconda3
    curl -sSL -O https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
    bash Miniconda3-latest-Linux-x86_64.sh -p /opt/miniconda3 -b
    rm -fr Miniconda3-latest-Linux-x86_64.sh
    export PATH=/opt/miniconda3/bin:$PATH
    conda update -n base conda
    conda config --add channels conda-forge
    conda config --add channels bioconda

    # install the R programming language
    conda install --yes -c conda-forge r-base==3.5.1

    # install some dependencies to build R packages
    apt-get -y install build-essential gfortran

    # install R bioconductor including DESeq2
    Rscript -e "source ('https://bioconductor.org/biocLite.R'); biocLite(c('ape', 'pegas', 'adegenet', 'phangorn', 'sqldf', 'ggtree', 'ggplot2', 'phytools', 'DESeq2'))"

    # install some bioinfo tools from Bioconda
    conda install --yes -c bioconda bedtools
    conda install --yes -c bioconda bowtie
    conda install --yes -c bioconda bowtie2
    conda install --yes -c bioconda bwa
    conda install --yes -c bioconda htseq
    conda install --yes -c bioconda macs2
    conda install --yes -c bioconda samtools
    conda install --yes -c bioconda sra-tools
    conda install --yes -c bioconda blast

#install ucscapps from pre-compiled binaries they offer:
cd /opt
mkdir ucscapps
cd ucscapps
rsync -a -P rsync://hgdownload.soe.ucsc.edu/genome/admin/exe/linux.x86_64/ ./

#install javaGenomicsToolkit 
cd /opt
git clone https://github.com/timpalpant/java-genomics-toolkit.git
cd java-genomics-toolkit
ant

%environment
    export LANG=en_US.UTF-8
    export LANGUAGE=en_US:en
    export LC_ALL=en_US.UTF-8
    export XDG_RUNTIME_DIR=""
    export PATH=/opt/miniconda3/bin:$PATH
    export PATH=$PATH:/opt/ucscapps:/opt/ucscapps/blat:/opt/java-genomics-toolkit


