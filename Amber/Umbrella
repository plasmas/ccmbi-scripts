#!/bin/bash

######################################
# Define text colors
NC='\033[0m'
Red='\033[0;31m'
Green='\033[0;32m'
Yellow='\033[1;33m'
Blue='\033[0;34m'
Purple='\033[0;35m'
LightBlue='\033[0;36m'
######################################

stty erase '^H'

function Check_Answer(){
  case $1 in
  $2)  echo "";break 2;;
  $3)  echo "";break 1;;
   *)  echo "Error Input!Just input $2 or $3!";;
  esac
}
# This function is used to judge custom input
# Usage: Check_Answer $ANSWER Option1 Option2
# If the answer equils to option1 you will break out 2 loops
# If the answer equils to option2 you will break out 1 loop
function Create_Folders(){
  mkdir ANA 2>/dev/null #This dir is created to do the analysis after MD
  touch ANA/ana.dat 2>/dev/null #Prepare for WHAM
  mkdir DATA 2>/dev/null #This dir is created to save datas made by Ubrella Sampling
  mkdir MD  2>/dev/null #MD Folder
    mkdir MD/IN 2>/dev/null #This dir is created to save .in files for MD
    mkdir MD/OUT 2>/dev/null #This dir is created to save .rst and .out files for MD
}
# This function is used to create folders for whole umbrella sampling experiment
# Create folders (ANA DATA MD/IN MD/OUT) and file (ana.dat)
function Create_Fixed_Files(){
  #=======================================================================================================================#
  echo "5000 step minimization"                                                                     > MD/IN/min.in
  echo "&cntrl"                                                                                    >> MD/IN/min.in
  echo "  imin = 1,maxcyc=5000,ncyc = 2000,irest = 0,ntx = 1,"                                     >> MD/IN/min.in
  echo "  ntpr = 1000,iwrap=1,cut = 10.0,nmropt = 1,"                                              >> MD/IN/min.in
  echo "&end"                                                                                      >> MD/IN/min.in
  echo "&wt"                                                                                       >> MD/IN/min.in
  echo "  type='END',"                                                                             >> MD/IN/min.in
  echo "&end"                                                                                      >> MD/IN/min.in
  echo "DISANG=MD/IN/parm.RST"                                                                     >> MD/IN/min.in
  #=======================================================================================================================#
  echo "50 ps NPT equilibration"                                                                    > MD/IN/equil.in
  echo "&cntrl"                                                                                    >> MD/IN/equil.in
  echo "  imin = 0,ntx = 1,irest = 0,ntpr = 1000,ntwr = 1000,ntwx = 0,iwrap=1,"                    >> MD/IN/equil.in
  echo "  ntf = 2,ntc = 2,cut = 10.0,ntb = 2,nstlim = 50000,dt = 0.001,temp0 = 300,"               >> MD/IN/equil.in
  echo "  ntt = 3,ig=-1,gamma_ln = 4.0,ntp = 1,pres0 = 1.0,nmropt = 1,"                            >> MD/IN/equil.in
  echo "&end"                                                                                      >> MD/IN/equil.in
  echo "&wt"                                                                                       >> MD/IN/equil.in
  echo "  type='END',"                                                                             >> MD/IN/equil.in
  echo "&end"                                                                                      >> MD/IN/equil.in
  echo "DISANG=MD/IN/parm.RST"                                                                     >> MD/IN/equil.in
  #=======================================================================================================================#
  echo "0.1 ns NPT production"                                                                      > MD/IN/md.in
  echo "&cntrl"                                                                                    >> MD/IN/md.in
  echo "  imin = 0,ntx = 5,irest = 1,iwrap=1,ntpr = 1000,ntwx = 4000,ntf = 2,ntc = 2,"             >> MD/IN/md.in
  echo "  cut = 10.0,ntb = 2,nstlim = 100000,dt = 0.001,temp0 = 300.0,ntt = 3,gamma_ln = 4.0,"     >> MD/IN/md.in
  echo "  ig = -1,ntp = 1,pres0 = 1.0,nmropt = 1,"                                                 >> MD/IN/md.in
  echo "&end"                                                                                      >> MD/IN/md.in
  echo "&wt"                                                                                       >> MD/IN/md.in
  echo "  type='DUMPFREQ',istep1=50,"                                                              >> MD/IN/md.in
  echo "&end"                                                                                      >> MD/IN/md.in
  echo "&wt"                                                                                       >> MD/IN/md.in
  echo "  type='END',"                                                                             >> MD/IN/md.in
  echo "&end"                                                                                      >> MD/IN/md.in
  echo "DISANG=MD/IN/parm.RST"                                                                     >> MD/IN/md.in
  echo "DUMPAVE=MD/OUT/data.out"                                                                   >> MD/IN/md.in
  #=======================================================================================================================#
}
# This function is used to create fixed files
# min.in | equil.in | md.in
function Get_Parameter_Informations(){
  PARAMETERS=(2 0 0 0 0 0 0 0 2 0 0 0 0 0 0 0)
  while [ True ]; do
    echo    "  |   |-- 3.3 How many groups of Parameters do you need[1/2]?"
    read -p "  |   |   \`-- " -n1 LOOPNUMBER
    Check_Answer $LOOPNUMBER 1 2
  done
  for (( i = 1; i <= $LOOPNUMBER; i++ )); do
    echo   "  |   |       |                                                     "
    while [ True ]; do
      echo "  |   |       |-- Give me your group[$i] of parameters(5/7)"
      echo "  |   |       |   ATOM1 ATOM2 [ATOM3 ATOM4] MAXSTART STEPLENGTH STEPNUMBER:"
      read -p  "  |   |       |   |-- " PARA1 PARA2 PARA3 PARA4 PARA5 PARA6 PARA7
      if [ -n "$PARA7" ]; then
        PARAMETERS[8*$i-8]=Ppi
        PARAMETERS[8*$i-7]=$PARA1
        PARAMETERS[8*$i-6]=$PARA2
        PARAMETERS[8*$i-5]=$PARA3
        PARAMETERS[8*$i-4]=$PARA4
        PARAMETERS[8*$i-3]=$PARA5
        PARAMETERS[8*$i-2]=$PARA6
        PARAMETERS[8*$i-1]=$PARA7
        echo "  |   |       |   |-- You choose Angle($PARA1,$PARA2,$PARA3,$PARA4) model to do umbrella sampling"
        while [ True ];do
          read -s -p "  |   |       |   |   from $PARA5 to $(echo "$PARA5-$PARA6*$PARA7" | bc) in $PARA7 steps  [Y/N]?" -n1 ANSWER
          Check_Answer $ANSWER Y N
        done
      elif [ -z "$PARA6" -a -n "$PARA5" ]; then
        PARAMETERS[8*$i-8]=Pval
        PARAMETERS[8*$i-7]=$PARA1
        PARAMETERS[8*$i-6]=$PARA2
        PARAMETERS[8*$i-5]=0
        PARAMETERS[8*$i-4]=0
        PARAMETERS[8*$i-3]=$PARA3
        PARAMETERS[8*$i-2]=$PARA4
        PARAMETERS[8*$i-1]=$PARA5
        echo "  |   |       |   |-- You choose Distance($PARA1,$PARA2) model to do umbrella sampling"
        while [ True ];do
          read -s -p "  |   |       |   |   from $PARA3 to $(echo "$PARA3-$PARA4*$PARA5" | bc) in $PARA5 steps [Y/N]?" -n1 ANSWER
          Check_Answer $ANSWER Y N
        done
      else
      echo "  |   |       |   \`-- Error Parameter Input! You need to give me 5 or 7 parameters!"
      fi
    done
    echo    "  |   |       |   \`-- Group[$i] parameters collection: Complete!   "
  done
  echo      "  |   |       |                                                  "
  echo      "  |   |       \`-- Complete!                                     "
}
# This function is used to get parameter informations
# It can get ATOMNUMBERS,MAXSTART,STEPLENGTH,STEPNUMBER informations in an array
# PARAMETERS = (Flag1,Atom1,Atom2,[Atom3,Atom4,]MAXSTART1,STEPLENGTH2,STEPNUMBER3,
#               Flag2,Atom5,Atom6,[Atom7,Atom8,]MAXSTART1,STEPLENGTH2,STEPNUMBER3)
function Create_Custom_Files(){
  #=======================================================================================================================#
  echo "Umbrella Sampling Parameter for MD"                                                         > MD/IN/parm.RST
  if [[ ${PARAMETERS[0]} = Ppi ]]; then
      echo "&rst  iat=${PARAMETERS[1]},${PARAMETERS[2]},${PARAMETERS[3]},${PARAMETERS[4]},r1=0.000,r2=${PARAMETERS[5]},r3=${PARAMETERS[5]},r4=99.000,rk2=200,rk3=200,/"  >> MD/IN/parm.RST
    elif [[ ${PARAMETERS[0]} = Pval ]]; then
      echo "&rst  iat=${PARAMETERS[1]},${PARAMETERS[2]},r1=0.000,r2=${PARAMETERS[5]},r3=${PARAMETERS[5]},r4=99.000,rk2=200,rk3=200,/"                >> MD/IN/parm.RST
  fi
  if [[ ${PARAMETERS[8]} = Ppi ]]; then
      echo "&rst  iat=${PARAMETERS[9]},${PARAMETERS[10]},${PARAMETERS[11]},${PARAMETERS[12]},r1=0.000,r2=${PARAMETERS[13]},r3=${PARAMETERS[13]},r4=99.000,rk2=200,rk3=200,/"  >> MD/IN/parm.RST
    elif [[ ${PARAMETERS[8]} = Pval ]]; then
      echo "&rst  iat=${PARAMETERS[9]},${PARAMETERS[10]},r1=0.000,r2=${PARAMETERS[13]},r3=${PARAMETERS[13]},r4=99.000,rk2=200,rk3=200,/"             >> MD/IN/parm.RST
  fi
  #=======================================================================================================================#

  #=======================================================================================================================#
  echo "#!/bin/bash"                                                                                > Submit.sh
  echo "#PBS -N US_name"                                                                           >> Submit.sh
  echo "#PBS -q gpu"                                                                               >> Submit.sh
  echo "#PBS -l nodes=2:ppn=12"                                                                    >> Submit.sh
  echo "#PBS -j oe"                                                                                >> Submit.sh
  echo "PRMTOP=\"$TOP_FILE_NAME\""                                                                 >> Submit.sh
  echo "INPCRD=\"$CRD_FILE_NAME\""                                                                 >> Submit.sh
  echo "cd "'$'"PBS_O_WORKDIR"                                                                     >> Submit.sh
  echo "tmp1="'$'"(sed -n '2p' MD/IN/parm.RST | awk -F \"=\" '{print "'$'"1\"=\""'$'"2\"=\""'$'"3}')"    >> Submit.sh
  echo "tmp3="'$'"(sed -n '2p' MD/IN/parm.RST | awk -F \"=\" '{print "'$'"6\"=\""'$'"7\"=\""'$'"8}')"    >> Submit.sh
  if [[ $LOOPNUMBER = 1 ]]; then
    echo "for((i=0;i<=${PARAMETERS[7]};i++))"                                                      >> Submit.sh
    echo "  do"                                                                                    >> Submit.sh
    echo "  n="'$'"(printf "'%'"02d" '$'"i)"                                                       >> Submit.sh
    echo "  pmemd.cuda -O -p "'$'"PRMTOP -c "'$'"INPCRD      -i MD/IN/min.in   -o MD/OUT/min.out   -r MD/OUT/min.rst    -ref "'$'"INPCRD"        >> Submit.sh
    echo "  pmemd.cuda -O -p "'$'"PRMTOP -c MD/OUT/min.rst   -i MD/IN/equil.in -o MD/OUT/equil.out -r MD/OUT/equil.rst  -ref MD/OUT/min.rst"     >> Submit.sh
    echo "  pmemd.cuda -O -p "'$'"PRMTOP -c MD/OUT/equil.rst -i MD/IN/md.in    -o MD/OUT/md.out    -r MD/OUT/md.rst     -x MD/OUT/md"'$'"{n}.nc" >> Submit.sh
    echo "  cp MD/OUT/data.out DATA/data"'$'"{n}.dat"                                              >> Submit.sh
    echo "  next2="'$'"(echo \"${PARAMETERS[5]}-("'$'"i+1)*${PARAMETERS[6]}\"|bc)"                 >> Submit.sh
    echo "  tmp2="'$'"(printf \"="'$'"{next2},r3="'$'"{next2},r4=\")"                              >> Submit.sh
    echo "  sed -i \"2c "'$'"tmp1"'$'"tmp2"'$'"tmp3\" MD/IN/parm.RST"                                    >> Submit.sh
    echo "done"                                                                                    >> Submit.sh
  fi
  if [[ $LOOPNUMBER = 2 ]] ; then
    echo "tmp4="'$'"(sed -n '3p' MD/IN/parm.RST | awk -F \"=\" '{print "'$'"1\"=\""'$'"2\"=\""'$'"3}')"    >> Submit.sh
    echo "tmp6="'$'"(sed -n '3p' MD/IN/parm.RST | awk -F \"=\" '{print "'$'"6\"=\""'$'"7\"=\""'$'"8}')"    >> Submit.sh
    echo "for((i=0;i<${PARAMETERS[7]};i++))"                                                       >> Submit.sh
    echo "  do"                                                                                    >> Submit.sh
    echo "  next2="'$'"(echo \"${PARAMETERS[5]}-("'$'"i+1)*${PARAMETERS[6]}\"|bc)"                 >> Submit.sh
    echo "  tmp2="'$'"(printf \"="'$'"{next2},r3="'$'"{next2},r4=\")"                              >> Submit.sh
    echo "  sed -i \"2c "'$'"tmp1"'$'"tmp2"'$'"tmp3\" MD/IN/parm.RST"                              >> Submit.sh
    echo "  for((j=0;j<${PARAMETERS[15]};j++))"                                                    >> Submit.sh
    echo "    do"                                                                                  >> Submit.sh
    echo "    n="'$'"(printf "'%'"02d"'%'"02d "'$'"i "'$'"j)"                                      >> Submit.sh
    echo "    next1="'$'"(echo \"${PARAMETERS[13]}-("'$'"j+1)*${PARAMETERS[14]}\"|bc)"             >> Submit.sh
    echo "    tmp5="'$'"(printf \"="'$'"{next1},r3="'$'"{next1},r4=\")"                            >> Submit.sh
    echo "    sed -i \"3c "'$'"tmp4"'$'"tmp5"'$'"tmp6\" MD/IN/parm.RST"                            >> Submit.sh
    echo "    pmemd.cuda -O -p "'$'"PRMTOP -c "'$'"INPCRD      -i MD/IN/min.in   -o MD/OUT/min.out   -r MD/OUT/min.rst    -ref "'$'"INPCRD"        >> Submit.sh
    echo "    pmemd.cuda -O -p "'$'"PRMTOP -c MD/OUT/min.rst   -i MD/IN/equil.in -o MD/OUT/equil.out -r MD/OUT/equil.rst  -ref MD/OUT/min.rst"     >> Submit.sh
    echo "    pmemd.cuda -O -p "'$'"PRMTOP -c MD/OUT/equil.rst -i MD/IN/md.in    -o MD/OUT/md.out    -r MD/OUT/md.rst     -x MD/OUT/md"'$'"{n}.nc" >> Submit.sh
    echo "    cp MD/OUT/data.out DATA/data"'$'"{n}.dat"                                            >> Submit.sh
    echo "  done"                                                                                  >> Submit.sh
    echo "done"                                                                                    >> Submit.sh
  fi
  #=======================================================================================================================#
}
# This function is used to create custom files
# Create files with parameters get from Get_Parameter_Informations() function
function Create_And_Submit() {
  printf  "${Yellow}==========================================================================================\n"
  echo    "                              Prepering for Ubrella Sampling                              "
  echo    "=========================================================================================="
  printf  "${NC}  |-- STEP1: Create Folders\n"
  Create_Folders
  printf  "  |   \`-- STEP1 ${Green}Complete!${NC}\n"
  echo    "  |                                                         "
  echo    "  |-- STEP2: Create Fixed Files                             "
  Create_Fixed_Files
  printf  "  |   \`-- STEP2 ${Green}Complete!${NC}\n"
  echo    "  |                                                         "
  echo    "  |-- STEP3: Create Custom Files                            "
  echo    "  |   |-- 3.1 Give me your .prmtop File's name:             "
  read -p "  |   |   \`-- " TOP_FILE_NAME
  echo    "  |   |                                                     "
  echo    "  |   |-- 3.2 Give me your .inpcrd File's name:             "
  read -p "  |   |   \`-- " CRD_FILE_NAME
  echo    "  |   |                                                     "
  Get_Parameter_Informations
  Create_Custom_Files
  echo    "  |   |                                                     "
  printf  "  |   \`-- STEP3 ${Green}Complete!${NC}\n"
  echo    "  |"
  echo    "  \`-- STEP4: Submit Your Script                            "

  printf  "      |-- Your submit informations: "
  #qsub Submit.sh
  echo    "      |"
  printf  "      \`-- STEP4 ${Green}Complete!${NC}\n"
  printf  "${Yellow}==========================================================================================\n"
  echo    "                              Ubrella Sampling is now Running                             "
  printf  "==========================================================================================${NC}\n"
}

Create_And_Submit
