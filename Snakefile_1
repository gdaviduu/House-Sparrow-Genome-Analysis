##### Snakefile for snakemake BWA index and mapping #####

configfile: "config.yaml"

rule genotype_gvcfs:
    input:
        # gvcf files
        gvcf="calls/all.g.vcf",
        ref=config["reference"],
    output:
        "calls/all.vcf"
    log:
        "logs/gatk/genotypegvcfs.log"
    params:
        extra="",  # optional
        java_opts=config["java_mem_genotype_gvcfs"],
    run:
        shell(
            "gatk --java-options '{params.java_opts}' GenotypeGVCFs {params.extra} "
            "-V {input.gvcf} "
            "-R {input.ref} "
            "-O {output}"
        )


rule combine_gvcfs:
    input:
        # gvcf files
        gvcfs=list(map("calls/{}.g.vcf".format, config["samples"])),
        ref=config["reference"],
    output:
        "calls/all.g.vcf",
    log:
        "logs/gatk/combinegvcfs.log"
    params:
        extra="",  # optional
        java_opts=config["java_mem_combine_gvcfs"], 
    run:
        gvcfs = list(map("-V {}".format, input.gvcfs))
        shell(
            "gatk --java-options '{params.java_opts}' CombineGVCFs {params.extra} "
            "{gvcfs} "
            "-R {input.ref} "
            "-O {output}"
        )


rule haplotype_caller:
    input:
        # single bam file
        bam = "dedup/{sample}.dedup.bam",
        ref = config["reference"],
        fai = config["reference"] + ".fai",
        dict = os.path.splitext(config["reference"])[0] + ".dict",
        #dict = config["reference"] + ".dict",
    output:
        "calls/{sample}.g.vcf",
    log:
        "logs/gatk/haplotypecaller/{sample}.log"
    params:
        extra="",  # optional
        java_opts=config["java_mem_haplotype_caller"], 
    run:
        #if isinstance(bam, str):
        #    bam = [bam]
        #bam = list(map("-I {}".format, bam))
        shell(
            "gatk --java-options '{params.java_opts}' HaplotypeCaller {params.extra} "
            "-R {input.ref} "
            "-I {input.bam} "
            "-ERC GVCF "
            "-O {output}"
        )


#rule all_mapping:
#    input:
#        "{sample}.done"

rule done:
    input:
        "{sample}.dedup.bam",
        "{sample}.dedup.metrics.txt",
        "{sample}.dedup.bam.bai",
        "{sample}.dedup.bam.flagstat"
#    input:
#        "{sample}.bam.bai",
#        "{sample}.bam.flagstat"
#    input:
#        expand("{sample}.bam.bai", sample=config["sample"]),
#        expand("{sample}.bam.flagstat", sample=config["sample"])
    output:
        "{sample}.done"
    shell:
        "touch {sample}.done"

rule bam_flagstat:
    input:
        "{sample}.dedup.bam"
    output:
        "{sample}.dedup.bam.flagstat"
    params:
        samtools_threads = config["samtools_threads"]
    shell:
        """
        module load bioinfo-tools samtools/1.9
        samtools flagstat {params.samtools_threads} {input} > {output}
        """

rule bam_index:
    input:
        "{sample}.dedup.bam"
    output:
        "{sample}.dedup.bam.bai",
    params:
        samtools_threads = config["samtools_threads"]
    shell:
        """
        module load bioinfo-tools samtools/1.9
        samtools index {params.samtools_threads} {input}
        """

rule bam_markdups:
    input:
        "{sample}.bam"
    output:
        bam = "{sample}.dedup.bam",
        metrics = "{sample}.dedup.metrics.txt"
    params:
        "REMOVE_DUPLICATES=false"
    log:
        "{sample}.dedup.log",
    shell:
        """
        module load bioinfo-tools picard/2.20.4
        java -jar $PICARD_HOME/picard.jar MarkDuplicates {params} INPUT={input} OUTPUT={output.bam} METRICS_FILE={output.metrics} &> {log}
        """

rule bwa_index:
    """Index the reference genome"""
    input:
        ref = config["reference"]
    output:
        amb = config["reference"] + ".amb",
        ann = config["reference"] + ".ann",
        bwt = config["reference"] + ".bwt",
        pac = config["reference"] + ".pac",
        sa = config["reference"] + ".sa",
    threads: 1
    shell:
        """
        module load bioinfo-tools bwa/0.7.17
        bwa index {input.ref}
        """

rule fasta_dict:
    """Create an DICT file from a fasta file"""
    input:
        "{some}.fasta"
    output:
        "{some}.dict"
    threads: 1
    shell:
        """
        module load bioinfo-tools picard/2.20.4
        java -jar $PICARD_HOME/picard.jar CreateSequenceDictionary R={input} O={output}
        """

rule fasta_index:
    """Create an FAI index from a fasta file"""
    input:
        config["reference"]
    output:
        config["reference"] + ".fai"
    threads: 1
    shell:
        """
        module load bioinfo-tools samtools/1.9
        samtools faidx {input}
        """

rule bwa_mapping:
    """Map short reads to reference genome with BWA mem"""
    input:
        fastq_1 = config["path_to_reads"] + "{sample}_R1.trimmed.fastq.gz",
        fastq_2 = config["path_to_reads"] + "{sample}_R2.trimmed.fastq.gz",
        ref = config["reference"],
        fai = config["reference"] + ".fai",
        amb = config["reference"] + ".amb",
        ann = config["reference"] + ".ann",
        bwt = config["reference"] + ".bwt",
        pac = config["reference"] + ".pac",
        sa = config["reference"] + ".sa",
    params:
        bwa_threads = config["bwa_threads"],
        samtools_threads = config["samtools_threads"],
        readgroup = r"-R '@RG\tID:{sample}\tSM:{sample}'"
    output:
        bam = "{sample}.bam"
    log:
        bwa = "{sample}.bam.bwa.log",
        samtools = "{sample}.bam.samtools.log"
    threads: 1
    shell:
        """
        module load bioinfo-tools bwa/0.7.17 samtools/1.9
        bwa mem {params.bwa_threads} {params.readgroup} {input.ref} {input.fastq_1} {input.fastq_2} 2>{log.bwa} | samtools sort - {params.samtools_threads} -O BAM > {output.bam} 2>{log.samtools}
        """


