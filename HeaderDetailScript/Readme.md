## HeaderDetailScript
A script which is like the TABLEAUX script but it works with more than one detail its a little bit different to set up but this is the drawback fousing it with multiple details. Contributions to make it better are very welcome!  
I'm new to SAGE X3 and therfore there might be better solutions so please excuse my TODO comments and my code :-)

#### Info
To use the script you have to call it from some actions:  
```
LIENS       --> Call LIENS_DETAIL(PARM1, PARM2, PARM3) From SPEFWIHEADERDETAIL  
CREATION    --> Call CREATION_DETAIL(PARM2, PARM3) From SPEFWIHEADERDETAIL  
MODIF       --> Call MODIF_DETAIL(PARM1, PARM2, PARM3) From SPEFWIHEADERDETAIL   
ANNULE      --> Call ANNULE_DETAIL(PARM1, PARM2, PARM3) From SPEFWIHEADERDETAIL  
APRES_MODIF --> Call APRES_MODIF_DETAIL() From SPEFWIHEADERDETAIL 
```

due the script uses the default files and masks we have to add   
```
Default Mask [DETAILSCREEN]  
Default File [DETAILFILE]  
```
before each call of a subprog from the Header-Detail script (see example below)
And another thing to mention is that the script needs a "LIN" Table Field (line number of detail) and the same standard fields the TABLEAUX Script uses (CREFLG, UPDFLG, NBLIG).  
Unfortunately, to keep it simple, those fields are not renamable. That means their names are coded in the script. But of course you may adept the script so that they can be variable...

The Parameters of the subprogs (if they have one) are:  
```
PARM1 (char) --> the header-detail link (range the detail)  
PARM2 (char) --> SCRIPTNAME / the name of a script where the "callback" subprogs are triggered (see callback subprogs)  
PARM3 (char) --> SCRIPTID / a identifier for the callback subprogs (used if you have more than one detail in a script)
```

#### Callback subprogs
the HeaderDetail Script calls some subprogs (if they are defined in the PARAM2 Script)  
please look them up in the script itself. There are for example:
```
POST_LIENS(SCRIPTID)
POST_DELETE(SCRIPTID)
POST_CREATE(SCRIPTID)
VALIDATE_WRITE(SCRIPTID)
INIT_FIELDS(SCRIPTID)*

* those have to be defined!
```

#### Example of using with one detail
(For one Detail i'd suggest you to use the TABLEAUX Script because its better maintained)
```
# SCRIPT: SPEHEADERSCRIPT

$ACTION
  Case ACTION
   When "LIENS"       : Gosub $SET_DEFAULT : Call LIENS_DETAIL(func GET_PKRANGE(), 'SPEHEADERSCRIPT', '1') From SPEFWIHEADERDETAIL   
   When "CREATION"    : Gosub $SET_DEFAULT : Call CREATION_DETAIL('SPEHEADERSCRIPT', '1') From SPEFWIHEADERDETAIL                     
   When "MODIF"       : Gosub $SET_DEFAULT : Call MODIF_DETAIL(func GET_PKRANGE(), 'SPEHEADERSCRIPT', '1') From SPEFWIHEADERDETAIL    
   When "ANNULE"      : Gosub $SET_DEFAULT : Call ANNULE_DETAIL(func GET_PKRANGE(), 'SPEHEADERSCRIPT', '1') From SPEFWIHEADERDETAIL   
   When "APRES_MODIF" : Gosub $SET_DEFAULT : Call APRES_MODIF_DETAIL() From SPEFWIHEADERDETAIL                       
   When Default
  Endcase
Return

$SET_DEFAULT
  Default Mask [DETAILSCREEN]
  Default File [SETAILFILE]
Return

Subprog INIT_FIELDS(SCRIPTID)
  Value Char SCRIPTID
  [F]BPRNUM = [M:HEADERSCREEN]BPRNUM
End

Funprog GET_PKRANGE()
End 'BPRNUM="'+[M:HEADERSCREEN]BPRNUM+'"'
```

#### Example of using with two details in one script
```
# SCRIPT: SPEHEADERSCRIPT

$ACTION
  Case ACTION
   When "LIENS"       : Gosub $SET_DEFAULT  : Call LIENS_DETAIL(func GET_PKRANGE(), 'SPEHEADERSCRIPT', '1') From SPEFWIHEADERDETAIL
                        Gosub $SET_DEFAULT2 : Call LIENS_DETAIL(func GET_PKRANGE(), 'SPEHEADERSCRIPT', '2') From SPEFWIHEADERDETAIL
   When "CREATION"    : Gosub $SET_DEFAULT  : Call CREATION_DETAIL('SPEHEADERSCRIPT', '1') From SPEFWIHEADERDETAIL   
                        Gosub $SET_DEFAULT2 : Call CREATION_DETAIL('SPEHEADERSCRIPT', '2') From SPEFWIHEADERDETAIL
   When "MODIF"       : Gosub $SET_DEFAULT  : Call MODIF_DETAIL(func GET_PKRANGE(), 'SPEHEADERSCRIPT', '1') From SPEFWIHEADERDETAIL  
                        Gosub $SET_DEFAULT2 : Call MODIF_DETAIL(func GET_PKRANGE(), 'SPEHEADERSCRIPT', '2') From SPEFWIHEADERDETAIL
   When "ANNULE"      : Gosub $SET_DEFAULT  : Call ANNULE_DETAIL(func GET_PKRANGE(), 'SPEHEADERSCRIPT', '1') From SPEFWIHEADERDETAIL 
                        Gosub $SET_DEFAULT2 : Call ANNULE_DETAIL(func GET_PKRANGE(), 'SPEHEADERSCRIPT', '2') From SPEFWIHEADERDETAIL 
   When "APRES_MODIF" : Gosub $SET_DEFAULT  : Call APRES_MODIF_DETAIL() From SPEFWIHEADERDETAIL
                        Gosub $SET_DEFAULT2 : Call APRES_MODIF_DETAIL() From SPEFWIHEADERDETAIL                       
   When Default
  Endcase
Return

$SET_DEFAULT
  Default Mask [DETAILSCREEN]
  Default File [SETAILFILE]
Return

$SET_DEFAULT2
  Default Mask [DETAILSCREEN2]
  Default File [SETAILFILE2]
Return

Subprog INIT_FIELDS(SCRIPTID)
  Value Char SCRIPTID
  # due all of the details have the same pk link to the header, there is no need to check the SCRIPTID value
  # but you could do: If SCRIPTID = "1" : ... : Endif
  [F]BPRNUM = [M:HEADERSCREEN]BPRNUM
End

Funprog GET_PKRANGE()
End 'BPRNUM="'+[M:HEADERSCREEN]BPRNUM+'"'
```






