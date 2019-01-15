# CCMBI-Scripts
This repository stores useful and convenient scripts for CCMBI laboratory @SJTU.
The repository is maintained by WYQ@331.

### Script Usage
The scripts are generally categorized as following:
* Job manipulating -- submit, monitor, etc.
* QM/MM calculation -- extract converged configuration, etc.

### PBS/Clean
'Clean' is used to dump Gaussian trash. It's written in bash.
please type ./Clean -h to see more options.
Author: WYQ

### PBS/Submit
'Submit' is used to calibrate Gaussian input files and inject through qsub.
Wildcard currently not supported.
please type ./Submit -h to see more options.
Author: WYQ

### PBS/Monitor
'Monitor' is used to monitor PBS(qstat) server tasks. It's written in python.
Please use this script at your own discretion.
This script is NOT designed to use on a public server, especially for a public account.
because passwords are stored in plain text, or there are modules missing.
It is however, designed to run on your own linux server, or windows subsystems (WSLs).
You should edit the config part below and install missing modules before use.
Author: WYQ

## Contributing
Contributions are welcome, any members of CCMBI can upload a stable version of their custom script to this repository.
Before making a commit, check the following things:
* No sensitive information (username, hostname, password) in your script.
* A short description at the head of the script.
* Detailed annotations are highly encouraged.
