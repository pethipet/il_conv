<p>
  <img src="res/logo.png" width="20%">
</p>

# THIS IS A FORK please go back to the original upstream repository.
  ** https://github.com/Paul-Hi/il_conv **


# *New* Version: v2.3
- Fix:      #10 Correct mitigation when hovered over TCVX-keys
- Add:      #7 Resolved / check column added
  
**NOTE: The resolved / check column is not preserved during runs.**  
Might be a feature in future.

**[Release list](RELEASES.md)**

# **il_conv** (**I**nspector **L**og **conv**erter)
A small tool to generate Excel  report (xlsx file) enriched with all known issue information incl. direct links to latest information about each known issue from TASKING's issue portal.  
Please adapt the output to whatever format you need for your errata management approach you follow at your company. If you like you can propose new formats (issue and/or pull request) to our project and we are happy to integrate them.

## Pre-requisites
The tool requires certain input files to generate the a meaningfull report.  
You need
1. To be a user of a TASKING compiler with a valid license.
2. To be a user of the TASKING TriCore Inspector with a valid license.
3. To have login access to TASKING issue portal for the respective TASKING compiler version,
   - To export XML data file manually (Button: XML Export)  
     or
   - To frequently download the XML data file automatically, e.g. on Windows with
   > \> wget --save-cookies cookies.txt --keep-session-cookies --post-data "login_username=**ABCD@youraccount.com**&secretkey=**PASSWORD**" -O NUL "https://issues.tasking.com/cust_redirect.php"
     \>  
     \> wget --load-cookies cookies.txt --content-disposition "https://issues.tasking.com/?project=TCVX&version=v6.3r1&o=xml"  
     \> wget --load-cookies cookies.txt --content-disposition "https://issues.tasking.com/?project=TCVX&version=v6.2r2&o=xml"  
4. The release notes of your used TASKING Inspector version (e.g. 'readme_tricore_v6.3r1_inspector_v1.0r7.html').  
   You can find that in TASKING website or on the installation tree of the TASKING Inspector product.
5. To integrated and execute the TASKING Inspector 'compiler' within your build environment. During execution you have to gathered detection warnings from the analysis of your source code.  
   **Tip:** Option '--inspector-log=<absolute-filename>' passed to the Inspector compiler toolchain helps you to gather all relevant output in one file.
   
## Background:
TASKING Inspector products are modified TASKING C/C++ compiler. Inspector helps a compiler user to evaluate if he might be impacted by a know issue of the compiler and to decided if and how to mitigate the issue. Generally such an impact analyise takes place in all safety related system & software development to be confident that system/software already release or where a update is in-development can't lead to a a safety critical malfunction for the (end)-user.

Examples of a mitigation when potential issue is detect:
- Use a newer compiler version or latested patch version which most probably already have the detected issue fixed.
- Change command line option or enable / disable specific compiler option functionlity local to a function or file.
- Change / Modify source code to avoid triggering the known issue.
- Proof that potential malfunction of application area is acceptable, e.g. can't lead to a safety critical malfunction  
...

### How does it work?
TASKING customer run the Inspector 'compiler' like the normal compiler from TASKING and get an enriched log output which shows the location (issue detected, filename, line, column).

## Example usage:
```
$ il_conv -?
...
$ il_conv -v -x issues_tasking_TCVX_v6.3r1.xml -r readme_tricore_v6.3r1_inspector_v1.0r6.html logfile.txt
```
Note:  
- 'issues_tasking_TCVX_v6.3r1.xml               == XML export from the issue portal
- 'readme_tricore_v6.3r1_inspector_v1.0r7.html' == Release note for your Inspector version
- logfile.txt                                   == one or several file which have inspector detection messages gathered for you build.

## Features:
- [x] Command line tool
- [x] Can use all published information from TASKING issue portal (XML export to be done by user)
- [x] Use the latest information available for detection (from readme/release note of the respective Inspector)
- [x] Generate one XLSX workbook report with one cover sheet + 3 data sheets (compact, normal, extended) based on user feedback :-)
  - **compact:** File, Filepath (hidden), Issue ID, SIL, Fixed Compiler Version, Summary, Description (hidden), Mitigation (hidden), Lines, detection types (hidden)
  Note: multiple occurences of detection gets collapsed in one line (customer request)  
  - **normal:** File, Filepath (hidden), Issue ID, SIL, Fixed Compiler Version, Summary, Description (hidden), Mitigation (hidden), Line, Column, detection type (hidden) 
  - **extended:** same like normal but all visible beside detection type
- [x] Hover over Issue ID shows Mitigation if any
- [x] Remove superfluos output (duplicate impact locations) potentially generated by Inspector tool
- [x] Binary release possible (one file executable script) without own python installation
- [x] Click-through from XLSX Issue ID  

## Limitation: 
Note:
 - There might be several 'doublicate' detections (same filename, same column and line) or (same filename butdifferent file system access paths, same column and line).
 - **il_conv** tool tries to warn you about that fact and ask you to look more deep into your source structure that you do not require to analyse detections to many times.
 > WARN: Your project seems to have multiple times file 'Os_Timer.h' in different folders, you should use expanded format and looking at the full pathname within the report
 This is oftent detected when you include header files via relative different files path from the view of the compile unit. 
  
 - The resulting Excel reports therefor might include 
   - **Compact report** - doublicate number for the same file / row
   - **Normal, Extended** - doublicate detection in different rows
   
## Know bugs: 
- none :-)
  It's a pretty simple script, hack it yourself or create a nice feature / bug ticket for us and we might have a look :-d

## Dependencies (pip -r requirements.txt)
- Python 3.10 or later
- openpyxl
- Pillow
- beautifulsoup4
  - lxml 
- ~~requests~~
- ~~numpy~~
- ~~datatable~~

In case you wanna build the scripts into a onefile executabe
- nuitka based - supported (see create_nuitka_exe.bat on windows)

Be aware that nuitka builds depends on a bunch of addtional things to be installed in addtion (e.g. gcc compiler) on the build machine
--> much easier is using the provided exe file from our release page

Paul & Peter

**Note:** 
This script is as is, no notion of completeness or that it works under all circumstances ...
### We'll get not payed for that work it's just for fun, so please be polite.
