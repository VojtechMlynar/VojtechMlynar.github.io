Use this tcl command to generate project script: 
![[TCL commands#^023c52]]

**Then modify it to relative paths** ([[https://www.fpgadeveloper.com/2014/08/version-control-for-vivado-projects.html/ |source]]):
1. Replace these lines: 
```tcl
# Set the reference directory for source file relative paths 
set origin_dir "." 

# Set the directory path for the original project from where this script was exported 
set orig_proj_dir "[file normalize "$origin_dir/orig-project"]" 

# Create project 
create_project ${_xil_proj_name_} "/proj_ref_7020" -part xc7z020clg400-1
```
2. With:
```tcl
# Set the reference directory to where the script is 
set origin_dir [file dirname [info script]] 

# Create project 
create_project ${_xil_proj_name_} $origin_dir -part xc7z020clg400-1 
```
3. Replace all absolute paths
```tcl
D:/0_Git/RP_InvPot/impl/

#e.g. D:/0_Git/RP_InvPot/impl/proj/proj_ref_7020
```
4. With:
```tcl
$origin_dir/../../

#e.g. ../../proj/proj_ref_7020
```
5. Find line
```tcl
set file_obj [get_files -of_objects [get_filesets constrs_1] [list "*$file"]]
```
6. And delete the `*` in front of `$file` 