## HeaderDetailScript
A script which is like the TABLEAUX script but it works with more than one detail its a little bit different to set up but this is the drawback fousing it with multiple details. Contributions to make it better are very welcome!  
I'm new to SAGE X3 and therfore there might be better solutions so please excuse my TODO comments and my code :-)

#### Info
To use the script you have to call it from some actions:  
LIENS       --> Call LIENS_DETAIL(PARM1, PARM2, PARM3) From SPEFWIHEADERDETAIL  
CREATION    --> Call CREATION_DETAIL(PARM2, PARM3) From SPEFWIHEADERDETAIL  
MODIF       --> Call MODIF_DETAIL(PARM1, PARM2, PARM3) From SPEFWIHEADERDETAIL   
ANNULE      --> Call ANNULE_DETAIL(PARM1, PARM2, PARM3) From SPEFWIHEADERDETAIL  
APRES_MODIF --> Call APRES_MODIF_DETAIL() From SPEFWIHEADERDETAIL 

due the script uses the default files and masks we have to add  
Default Mask [DETAILSCREEN]  
Default File [DETAILFILE]  
before each call of a script subprog (see example below)

The Parameters of the subprogs (if they have one) are:
PARM1 (char) --> the header-detail link (range the detail)
PARM2 (char) --> the name of a script where the "callback" subprogs are triggered (see callback subprogs)
PARM3 (char) -->

#### callback subprogs


#### Example of using with one detail
(For one Detail i'd suggest you to use the TABLEAUX Script because its better maintained)


#### Example of using with two details with a script for each detail 






