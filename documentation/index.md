---
title: Documentation
description: Main documentation
permalink: /documentation/
nav_order: 1
---

### Accessing Data.

The dataset is hosted in hosted in the public S3 bucket
`arn:aws:s3:::ubc-sunflower-genome`. If you wish to retrieve a file
from the set, then you may access it using any compatible AWS S3
Client or over HTTPS.

    # Example1: using an HTTP client
    # replace PATH/TO/KEY with the pathname of the file you wish to retrieve.

    GET https://ubc-sunflower-genome.s3-us-west-2.amazonaws.com/PATH/TO/KEY

    # Example2: using the `aws cli` program:
	aws s3 cp s3://ubc-sunflower-genome/PATH/TO/KEY ./

From within a program you should use the supported AWS clients.
From python, for instance, you may use boto3:

1. create a virtual virtualenv with venv or virtualenv (optional)

       virtualenv -p python3 --prompt="(sunflower) " .venv

   _Note: if you don't have virtualenv, you can install it first with
        `pip install virtualenv`, or use the builtin module in python3,
	i.e. `python3 -m venv .venv`_

   _Note: If you're on an older distribution with a different default python3
        and/or you don't have root access to install packages,
        you can bootstrap its installation as a regular user with conda_

        ~/miniconda3/bin/conda env create -n python36 python=3.6
        source ~/miniconda3/bin/activate python36

        # inside the conda environment, you have python3.6
        (python36) $ pip install --upgrade pip
        (python36) $ pip install virtualenv

        # create a python3.6 virtual env
        (python36) $ virtualenv --prompt="(sunflower) " -p python3.6 .venv

        # from this point on you no longer need the conda environment.
        # a copy of the python3.6 runtime was added to the .venv virtualenv
        # folder
        source ~/miniconda3/bin/deactivate

1. activate env

       source .venv/bin/activate

1. install python dependencies (includes awscli tools)

       # optional, but recommended before you install deps:
       pip install --upgrade pip
	   pip install boto3 awscli

#### Setting up Credentials

1. Configure your AWS credentials. This is detailed [elsewhere](https://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html), but here's one way:

       mkdir -p ~/.aws

   Add a section in `~/.aws/config`.:
       [profile reprod]
       region=us-west-2
       output=json

   Update your credentials file `~/.aws/credentials` (section header syntax differs from config file):

       [sunflower]
       aws_access_key_id=AKIAIOSFODNN7EXAMPLE
       aws_secret_access_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

   It's also a good idea to `chmod go-rw ~/.aws/credentials`.

#### Setup

Assuming you've configured your S3 client, you can export the AWS_PROFILE
environment variable to pick the desired account. If this
is not custmized, the aws cli tools will use the default account.

       # example
       export AWS_PROFILE=sunflower

### Overall organization

All the files present in this dataset are the result of analysis of
the raw sequence source files, and all the source files we used are
made available to the public by this dataset. Some of the files are
unique to this dataset, and some (older ones) are also present on
NCBI's Short Read Archive.

The dataset also contains the results of several analyses. Read below
to understand the layout.

### Reference Genomes

The dataset includes 4 reference genomes (fasta format, with indexes) for Heliantus, for which
we use the following identifiers:

  - ha412v2
  - hanxrqv1

```
  ha412v2:  s3://ubc-sunflower-genome/references/HA412/genome/Ha412HOv2.0-20181130.fasta
  hanxrqv1: s3://ubc-sunflower-genome/references/HanXRQr1.0-20151230/genome/HanXRQr1.0-20151230.fa
```

### Callset (Cohort) Organization

The final result of the analyses are variant call sets (in VCF
format).  The callsets compare N different samples with respect to
each other and against a known reference genome.  The available groups are listed in
the `cohorts` subfolder of the bucket.  We have one cohort for the
moment, which encapsulates all of the samples, but we will uploading
more shortly.

Each cohort folder is specified with two manifest files, containing the list of sample ids,
and locations of the raw sequences compared in the set.

```
    # cohort folder (relative to top of bucket)
    cohorts/<grefid>/<cohortid>/samples.json
    cohorts/<grefid>/<cohortid>/sample-names.tsv
```

 - `sample-names.tsv`: The list of sample identifiers/names included in the cohort.
    One sample name per line. Blank lines and lines starting with `#` should be ignored.

 - `samples.json`: List the sequencing `runs` that are to be compared in the cohort. Each line
    is one separate document providing metadata about the run. Example line:

	```
    {"r1": ["s3://...HI.1699.005.Index_23.291C_R1.fastq.gz", {"md5": "f9bf7e9183fee15e05b057eb8121e200"}],
     "r2": ["s3://...HI.1699.005.Index_23.291C_R2.fastq.gz", {"md5": "a674b7d6fb1fd6485428c552d3638564"}],
     "runid": "HI.1699.005.Index_23.291C", "sample_name": "291C", "species": "UNKNOWN"
    }
    ```

**Currently available cohorts:**

  - `cohorts/ha412v2/wgs_all/`: All known samples compared in one bundle.
     - s3://ubc-sunflower-genome/cohorts/ha412v2/wgs_all/sample-names.tsv
     - s3://ubc-sunflower-genome/cohorts/ha412v2/wgs_all/samples.json

### Whole-Genome Sequences (ILLUMINA)

The sequences are illumina whole-genome-sequenced, paired-end. They
come in (compressed) fastq format, in either a gzip envelope, or the
NCBI short-read archive format (.sra). When the project started, not
all samples were available on the SRA, so we have been relying on
both formats.

_Note: regarding the presence of .sra files in the dataset._ NCBI is
the authoritative source for .sra files usually, but after signalling
some errors in the metadata of a few entries, NCBI has had to
reprocess the data of a few files, causing fastq files to be modified,
while keeping the same run ids. Depending when the .sra files are
downloaded, the same run ID might produce a different archive from
NCBI.  We include a copy of the archive files to permit full
reproducibility.

### Aligned Sequences

Runs are aligned to a reference genome to produce an aligned BAM, and
then aligned runs are subsequently grouped by sample name by a merge
operation.

- `cohorts/ha412v2/wgs_all/`:
  - `s3://ubc-sunflower-genome/cohorts/ha412v2/wgs_all/bams.tsv`


This file is in the following format:

```
SAMPLENAME	REFERENCE	OUTPUTURL
291C	ha412	s3://ubc-sunflower-genome/build/merge.1-291C-sha1_bed13750e080dbca19785553fe9fe45e6771e861/bunnies.transform-result.json
298C	ha412	s3://ubc-sunflower-genome/build/merge.1-298C-sha1_9bfe22ee12a8dd6bd8222f9922958eda6470a523/bunnies.transform-result.json
371C	ha412	s3://ubc-sunflower-genome/build/merge.1-371C-sha1_124564817a7b2ae929c2d54fd261b1e7089df2b6/bunnies.transform-result.json
442M	ha412	s3://ubc-sunflower-genome/build/merge.1-442M-sha1_4bb4074d8eadb4020b30635ddebdfd1764280059/bunnies.transform-result.json
...
```
Each of the OUTPUTURL points to a JSON file which describes the
various files produced in the transformation. The .bam (and associated index) will be
located in the same folder as the json file.


### Genotypes (GATK Genomic VCFs, i.e. .g.vcf)

Each aligned BAM is then fed through GATK's HaplotypeCaller to produce
a Genomic VCF (.g.vcf).

- `cohorts/ha412v2/wgs_all/`:
  - s3://ubc-sunflower-genome/cohorts/ha412v2/wgs_all/gvcfs.tsv

The files are in the following format:

```
SAMPLENAME	REFERENCE	OUTPUTURL
291C	ha412	s3://ubc-sunflower-genome/build/genotype.1-291C-AS1-sha1_8ced42a8e4da42435a72955288ec8cba54ba9898/bunnies.transform-result.json
298C	ha412	s3://ubc-sunflower-genome/build/genotype.1-298C-AS1-sha1_5ca396cd57528a4e0c80b32030fab59562f7e33b/bunnies.transform-result.json
371C	ha412	s3://ubc-sunflower-genome/build/genotype.1-371C-AS1-sha1_1320eab5ac0f8cb5bce5b572fc31c70e89c3cde4/bunnies.transform-result.json
...
```

Each of the OUTPUTURL points to a JSON file which describes the
various files produced in the transformation. The .g.vcf.gz (and
associated index) will be located in the same folder as the json file.


### Raw Short Variants

All the .g.vcf of a cohort are logically split in genomic windows of
1Mbp, and for each window, Gatk's GenotypeGVCFs is called to compare
the genotypes across the cohort. This produces a "raw" VCF file.
