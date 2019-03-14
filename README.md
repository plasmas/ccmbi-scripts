# CCMBI-Scripts
This repository stores useful and convenient scripts📑 for CCMBI laboratory🤼@SJTU.
The repository is currently maintained by WYQ🤪@331.

## Script Categories
The scripts are generally categorized as following:
* PBS folder: Job manipulating✒ -- submit, monitor🖥, etc.
* Gaussian folder: Gaussian calculation -- extract converged configuration, etc.
* MD folder: MD simulation -- umbrella☂ sampling, etc.

## PBS folder
### Clean
`Clean` is used to dump Gaussian trash. It's written in bash.
please type `./Clean -h` to see more options.

Features:
* Remove .gjf files, or .log files (watch out!⚠).
* Remove ALL gaussian related files.
* preserve .chk✅, report files or other files.

Author: WYQ

### Submit
`Submit` is used to calibrate Gaussian input files and inject through qsub.
please type `./Submit -h` to see more options.

Features:
* Appoint memory🤔 (`%mem`), processor number💠 (`%nprocs`), lindaworkers👷‍️, queue🚥.
* Generate shell scripts but not submit them.
* Native wildcard supported. ~~Dominate the clusters!~~

Known Issues:
* Only use this script to submit .gjf files in your working directory⚒.
* Do NOT include multiple `link0` sequences (e.g. `%nprocs`) in your .gjf. Otherwise, only the firsts will be read.

Author: WYQ

### Monitor
`Monitor` is adapted from WRF's JobState bash script. It's used to monitor on-going jobs. Use `./Monitor -h` to see full usage.

Features:
* Continuous monitoring and refresh for a given time period. (Pro tip: 💠`shownodes`💠 is integrated in the continuous mode)
* Show others' jobs, full or only somebody. ~~(See who's keeping the nodes busy!)~~
* Show jobs of a specific state🗽, `'R'`🏃‍, `'Q'`💤, etc.
* Show jobs on a specific queue, `'high'`🔺,`'middle'/'dpool'`🔹, `'low'`🔻, `'gpu'`📺, etc.

Authors: WRF & WYQ

### Monitor (deprecated)
`Monitor` is no longer developing because it is not designed to be out of the box📦.
`Monitor` is used to monitor PBS(qstat) server tasks. It's written in python🐍.
Please use this script at your own discretion.
This script is NOT designed to use on a public server, especially for a public account.
because passwords are stored in plain text, or there are modules missing.
It is however, designed to run on your own linux server, or windows subsystems (WSLs).
You should edit the config part below and install missing modules before use.

Author: WYQ

## Gaussian folder
### Energy
`Energy` is used to output extrapolated energy of a oniom scan job. It's written in bash.

Author: LiuLei, arranged by WRF

### Gout2gjf
`Gout2gjf` is used to convert gjf files out of a gaussian log file. Use `./Gout2gjf -h` to see full usage.

Features:
* Output all convergent gjf files of a scan job or final gjf of an opt job.
* Output all gjf of a gaussian log file.
* Output all gjf files of selected step. (Only in scan job)
* Output original gjf of this gaussian job. ~~(If someone deleted his input file...)~~

Authors: LiuLei, arranged by WRF

## Contributing
Contributions are welcome🤣, any members of CCMBI🤼 can upload a stable version of their custom script to this repository.

Before making a commit, check the following things:
* ❌No sensitive information (username👥, hostname🎯, password🗝) in your script.
* ✔A short description at the head of the script📜.
* ✔Detailed annotations✒ are highly encouraged.
