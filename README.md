# Group Project

RNA seq analysis

rule get_SRA_by_accession:
    """
    Retrieve a single-read FASTQ file from SRA (Sequence Read Archive) by run accession number.
    """
    output:
        "data/raw_internal/{sample_id}.fastq.gz"
    shell:
        """
        fastq-dump {wildcards.sample_id} -X 25000 --readids \
            --dumpbase --skip-technical --gzip -Z > {output}
        """

rule fastqc:
    """
    Run FastQC on a FASTQ file.
    """
    input:
        "data/raw_internal/{sample_id}.fastq.gz"
    output:
        "results/{sample_id}_fastqc.html",
        "intermediate/{sample_id}_fastqc.zip"
    shell:
        """
        # Run fastQC and save the output to the current directory
        fastqc {input} -q -o .
        # Move the files which are used in the workflow
        mv {wildcards.sample_id}_fastqc.html {output[0]}
        mv {wildcards.sample_id}_fastqc.zip {output[1]}
        """
