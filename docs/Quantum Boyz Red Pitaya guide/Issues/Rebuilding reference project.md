1. Clone Git repo [[https://github.com/VojtechMlynar/RedPitaya_InvertedPotential]]
2. Navigate folder `GITREPO/impl/proj/proj_ref_7020`
3. Copy script `proj_ref_7020_main.tcl` to another folder `GITREPO/impl/proj/YOURPROJ`
4. Run Vivado 2020.2
	1. If Tcl console is not open, open it: Upper toolbar -> Window -> Tcl console
5. Navigate `YOURPROJ` folder using command `cd absolutefolderpath/YOURPROJ`
6. Enter command `source proj_ref_7020_main.tcl`
7. Wait for the project to rebuild and open

Additional notes:
- You may need to add `GITREPO/impl/srcs/ips` as an [[Adding folder as IP repository|IP repository]] 