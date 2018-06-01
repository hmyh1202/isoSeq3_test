My simple test for isoSeq3

from https://github.com/PacificBiosciences/IsoSeq3/blob/master/README.md
=============================================

# step1 - CCS

## ccs sub-soft from smrt link v5.1.0
	ccs \
	--force \
	--minSnr 4 \
	--minLength 200 \
	--minPasses 1 \
	--minPredictedAccuracy 0.8 \
	--minZScore -999 \
	--maxDropFraction 0.8 \
	--numThreads 20 \
	--reportFile ccs_report.txt \ #output a CCS summary file
	--logLevel DEBUG \
	--logFile ccs_log.txt \       #output a CCS process log file
	--noPolish \
	test.subreads.bam \           #input a original subreads.bam of sequel
	ccs.bam                       #output CCS sequence file of BAM format
  
### output files
ccs.bam
ccs.bam.pbi
ccs_log.txt
ccs_report.txt
-----------------------------------------
# step2 - lima

easy to use from https://github.com/PacificBiosciences/IsoSeq3/releases/download/v0.7.2/isoseq3

     lima \
        ccs.bam \           #input CCS sequence file of step1
	primers.fasta \     #input a file containing primers of 5p and 3p
	demux.bam \         #output BAM#
	--isoseq  \
	--no-pbi  \
	--dump-clips     \
	--min-passes 1   \
	--min-length 200 \
	--chunk-size 20  \
	--num-threads 20
## NOTE:
       The true output name of demux.bam is demux.primer_5p--primer_3p.bam, which renamed by the name of primer
### output files
        demux.json
        demux.lima.report
        demux.primer_5p--primer_3p.subreadset.xml
        demux.lima.clips
        demux.lima.summary
#### primers.fasta
        demux.lima.counts
        demux.primer_5p--primer_3p.bam
### primers.fasta
        >primer_5p
        AAGCAGTGGTATCAACGCAGAGTACATGGGG

        >primer_3p
        AAGCAGTGGTATCAACGCAGAGTAC

---------------------------------------------

# step3 - cluster & polish (FAST and easy to use)
easy to use from https://github.com/PacificBiosciences/IsoSeq3/releases/download/v0.7.2/isoseq3

        isoseq3 cluster   \
        	demux.primer_5p--primer_3p.bam \  #input of step2
	        unpolished.bam \                  #output
        	--verbose \
	        --num-threads 20 \
	        --logFile cluster.log

### output files
        cluster.log
        unpolished.flnc.consensusreadset.xml
        unpolished.flnc.bam.pbi
        unpolished.flnc.bam
        unpolished.transcriptset.xml
        unpolished.bam.pbi
#### unpolished.bam
        unpolished.fasta
        unpolished.cluster

        isoseq3 polish \
        	unpolished.bam \      #input unpolished BAM file of cluster   
        	test.subreads.bam \   #input original subreads BAM file
        	polished.bam \        #output polished BAM
        	--verbose \
        	--num-threads 20 \
        	--logFile polish.log  #output plish LOG file

### output files
        polished.lq.fastq.gz
        polished.hq.fastq.gz
        polished.lq.fasta.gz
        polished.hq.fasta.gz
        polished.transcriptset.xml
        polished.bam.pbi
        polished.bam
        polish.log
