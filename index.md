---
permalink: /
title: UBC Sunflower Genome
nav_order: 0
last_edited: 2020-03-26
---

# UBC Sunflower Genome Dataset

This dataset is a catalogue of functionally important variation that
can be used to create better-adapted sunflower varieties
(e.g. developing breeding lines with improved drought tolerance) and
to study the genetic mechanisms driving evolution and adaptation.

Sunflowers are challenging plants to work with, in part because their
genome is large (3.6 billion base pairs), exceeding that of humans by
300 million base pairs, but also because the genomes of different
sunflower lines differ in gene content and genome structure. This
dataset is the result of hundreds of thousands of vcpu hours.

To the best of our knowledge, as of this writing, this is the largest
genomic dataset for sunflower in the world. It is composed of
whole-genome-shotgun sequence data from 2,392 diverse lines of
cultivated, wild, and landrace sunflowers.

## What can be found in this set?

This dataset allows the genetic study of the germplasm (i.e. seed) of
2392 Sunflowers (a mix of wild, landrace, and cultivated varieties),
from multiple species, collected from diverse ecosystems throughout
North America. Plants were grown in a common garden experiment (in
greenhouse), and then whole-genome-sequenced over a total of 3523
Illumina HiSeq runs. All the plants involved are associated with
curated metadata in NCBI's Short Read Archive (SRA), where the raw
source data is currently available. Along with our analyzed dataset,
we also have phenotypic data and environmental metrics which will
accompany this dataset.

The genetic data is available in common data formats used in
bioinformatics (fastq, bam, .g.vcf, vcf).  Scripts and programs used
to produce the datase will be made available in a public git repository.

The current dataset consists in sunflower sequences aligned to the
best genome reference currently available for sunflower (Ha412HO
v2). The dataset will be grown to include variant calls and filtered
datasets (preserving only high-quality variants).

## How was this dataset produced?

The plant collection, and creation of the raw dataset is the fruit of
an intense several years of collaborative effort by members of the
Rieseberg Lab (See the [Contact page](/contact/)).

The raw data was processed with a novel open-source framework for
reproducibility developed at UBC, called ["Bunnies"](https://github.com/rieseberglab/bunnies). Bunnies allows developers to
express their data workflows in a reproducible way.

The data files produced by the framework however, are presented in
standard formats, and can be utilized by existing applications. You
may freely browse the files from the dataset and extract the
portions that are of interest to your study.

The framework is similar in many ways to existing frameworks and
languages (Airflow, Snakemake, nextflow.io, luigi, WDL, etc.), with
the difference that different researchers can reuse each other's
partial results. Bunnies captures the provenance of each data item,
and will reuse previous computed files if they are available, while
ensuring that the files are equivalent (exactly the same
parameters).Data-flow-graphs (e.g. makefiles) written with bunnies can
use this dataset as a cache of "partial results" for any downstream
analysis.

One of the goals with the This workflow will greatly reduce the amount
of time spent on bioinformatic analyses for members working
downstream, as they will be able to efficiently share and find
previously produced results.
