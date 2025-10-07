## 🗂️ How Nextflow Components Are Structured

### 1. Processes

Defined as blocks inside the .nf script.
Each process encapsulates a task (e.g., running FastQC, trimming reads).
Example:

```
process fastqc {
  input:
  file reads from fastq_files

  output:
  file "*.html" into qc_results

  script:
  """
  fastqc $reads
  """
}
```

### 2. Channels

Also defined in the same .nf file.
They are used to pass data between processes.
Example:

```
fastq_files = Channel.fromPath('data/*.fastq')
```

### 3. Operators

Used to transform or filter channels.
Example:

```
filtered_files = fastq_files.filter { it.name.endsWith('.fastq') }
```

### 4. Configurations

Typically placed in a separate file called nextflow.config.
This file defines resources, containers, and execution profiles.
Example nextflow.config:

```
process {
  cpus = 4
  memory = '8 GB'
  container = 'biocontainers/fastqc'
}

profiles {
  local {
    executor = 'local'
  }
  cluster {
    executor = 'slurm'
  }
}
```

note: check all the avail biocontainers: https://hub.docker.com/u/biocontainers

biocontainers/fastqc is a pre-built container that includes several software and all their dependencies.
When you run your workflow, Nextflow will automatically download this container if it's not already available on your system.
This ensures that your process runs in a consistent and reproducible environment, regardless of your local setup.
For our cloud computing, we may need to 

### Summary

| Component | Location | Purpose |
|---|---|---|
| Process | main.nf | Defines a task |
| Channel | main.nf | Passes data between tasks |
| Operator | main.nf | Transforms channel data |
| Configuration | nextflow.config | Sets resources, containers, profiles |
