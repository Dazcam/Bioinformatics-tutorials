```R
configfile: '../config/config.yaml'

rule all:
    input:
	      expand("../results/01_process_fq/{sample}.fastq", sample = config['samples'])

rule process_fastqs:
    input:  "../resources/fq/{sample}.fastq"
    output: "../results/01_process_fq/{sample}.fastq"
    shell:
            """

            cat {input} > {output}
            printf "\nfq processed.\n" >> {output}

            """

rule align_fastqs:
    input:  "../resources/fq/{sample}.fastq"
    output: "../results/01_process_fq/{sample}.fastq"
    shell:
            """

            cat {input} > {output}
            printf "\nfq processed.\n" >> {output}

            """

```
