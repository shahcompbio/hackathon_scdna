
# setup single cell pipeline locally

### create working dir

```
mkdir HACKATHON
cd HACKATHON
```


### Download refdata


```
wget https://hackathondatascp.blob.core.windows.net/data/refdata.tar

tar -xvf refdata
```


### Download data

```
wget https://hackathondatascp.blob.core.windows.net/data/data.tar

tar -xvf data
```


### create input yaml file

save this file: 
```
SA1044-A90707B-R03-C39:
  bam: data/bwa-aln/SA1044-A90707B-R03-C39.bam
  column: 39
  condition: B
  fastqs:
    AHLLH2AFXX:
      fastq_1: data/SA1044-A90707B-R03-C39_S524_R1_001.fastq.gz
      fastq_2: data/SA1044-A90707B-R03-C39_S524_R2_001.fastq.gz
      sequencing_center: UBCBRC
      sequencing_instrument: N550
  img_col: 34
  index_i5: i7-39
  index_i7: i5-03
  pick_met: C31
  primer_i5: AACCGGTT
  primer_i7: ACGTACGT
  row: 3
  sample_type: 'null'
SA1044-A90707B-R04-C41:
  bam: data/bwa-aln/SA1044-A90707B-R04-C41.bam
  column: 41
  condition: B
  fastqs:
    AHLLH2AFXX:
      fastq_1: data/SA1044-A90707B-R04-C41_S466_R1_001.fastq.gz
      fastq_2: data/SA1044-A90707B-R04-C41_S466_R2_001.fastq.gz
      sequencing_center: UBCBRC
      sequencing_instrument: N550
  img_col: 32
  index_i5: i7-41
  index_i7: i5-04
  pick_met: C31
  primer_i5: AACCGGTT
  primer_i7: ACGTACGT
  row: 4
  sample_type: 'null'
```


### docker options

save as context_config.yaml

```

docker:
    server: 'docker.io'
    username: null
    password: null
    mounts:
      home: /home
```

NOTE: mounts should list the directory with all refdata.



### launch pipeline:


#### alignment:
```
docker run -w $PWD -v $PWD:$PWD -v /var/run/docker.sock:/var/run/docker.sock \
-v /usr/bin/docker:/usr/bin/docker --rm  \
singlecellpipeline/single_cell_pipeline:v0.5.6 single_cell alignment \
--input_yaml align.yaml --library_id A97318A --maxjobs 2 \
--context_config context_config.yaml --submit local \
--loglevel DEBUG --tmpdir temp/temp --pipelinedir temp/pipeline \
--out_dir results/metrics --bams_dir results/bams \
--config_override '{"refdir":"refdata/refdata"}'
```

