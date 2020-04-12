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
to understand the layout..

#### Reference Genomes

The dataset includes 4 reference genomes (fasta format, with indexes) for Heliantus, for which
we use the following identifiers:

  - ha412v2
  - hanxrqv1

```
  ha412v2:  s3://ubc-sunflower-genome/references/HA412/genome/Ha412HOv2.0-20181130.fasta
  hanxrqv1: s3://ubc-sunflower-genome/references/HanXRQr1.0-20151230/genome/HanXRQr1.0-20151230.fa
```

#### Callset (Cohort) Organization

The final result of the analyses are variant call sets (in VCF
format).  The callsets compare N different samples with respect to
each other and against a known reference genome.  The available groups are listed in
the `cohorts` subfolder of the bucket.  We have one group for the
moment, which encapsulates all of the samples, but we will uploading
more shortly.

Each group starts with two manifest files, containing the list of sample ids,
and locations of the raw sequences compared in the set.

```
	# hierarchy
    cohorts/<grefid>/<cohortid>/samples.json
	cohorts/<grefid>/<cohortid>/sample-names.tsv
```

 - `sample-names.tsv`: The list of sample identifiers/names included in the cohort.
    One sample name per line. Blank lines and lines starting with `#` should be ignored.

 - `samples.json`: List the sequencing runs that are to be compared in the cohort. Each line
    is one separate document providing metadata about the run. Example line:
    
	```{"r1": ["s3://ubc-sunflower-genome/sequence/projects/10090/HI.1699.005.Index_23.291C_R1.fastq.gz", {"md5": "f9bf7e9183fee15e05b057eb8121e200"}],
	    "r2": ["s3://ubc-sunflower-genome/sequence/projects/10090/HI.1699.005.Index_23.291C_R2.fastq.gz", {"md5": "a674b7d6fb1fd6485428c552d3638564"}],
		"runid": "HI.1699.005.Index_23.291C", "sample_name": "291C", "species": "UNKNOWN"
	   }
    ```

#### Whole-Genome Sequences (ILLUMINA)

The sequences are illumina whole-genome-sequenced, paired-end. They
come in (compressed) fastq format, in either a gzip envelope, or the
NCBI short-read archive format (.sra). When the project started, not
all samples were available on the SRA, so we have been relying on
both formats.

On a couple occasions, we have discovered errors in the metadata
processing performed by the SRA. Some of the SRA entries have been to
re-processed (and sra files in the archive have been updated), while
keeping the same run ids. The metadata manifests in this dataset
indicate the SRA run ids (i.e. `SRRXXX...`) when available, but we
also include a copy of the archive files that we downloaded at the
time the analysis started, to permit full reproducibility.

#### Aligned Sequences

#### Genotypes (GATK Genomic VCFs, i.e. .g.vcf)

#### Short Variants

### Choosing Samples.

### Next Steps
