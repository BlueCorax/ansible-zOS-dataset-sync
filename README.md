# ansible-zOS-dataset-sync
Import / Export multiple datasets from z/OS with Ansible

## Notes
I heard from COBOL Devs and z/OS engineers that they often run into the problem that they need JCLs on other z/OS instances. However copying the necessary datasets manually is not very pleasant and a Rexx script is often too complex / requires too much customization with each move.
My solution was to create ansible playbook which make it very easy to archive and  import / export a number of ds and pds. With a few central adjustments in the playbook you can export and import everything you need.

Never again will you have to think "Where the hell was this JCL again?"

As a student that works part-time as a COBOL developer, this would be a very welcome utility if it were available at my workplace!

## Instructions
### Requirements
- The docker image masterthemainframe/ansible:latest contains all the necessary software and ansible modules.

### Steps
1. Specify hosts in the hosts file. The host mtm will be used for the export and the hosts under import_mtm will be used for the import.
2. export.yml: specify the datasets to export in the list "datasets" found under "vars"
2. Run the ansible script export.yml. Provide the password when prompted. The result should be an archive file named export_<date>.tar
3. (optional) Open the archive and edit if adjustments are needed.
4. import.yml: specify the archive_path of the archive that was previously exported
5. Run the ansible script import.yml. Provide the password when prompted. Your datasets have been imported.

The playbooks are well structured and easily expandable with more features such as automatic text replacements during import and export or multi-user setups. The playbooks could also easily be merged together to export and import to other systems in one execution.
A big advantage of this solution is that it enables you to have a single source of truth when working across multiple systems. Just add more hosts to the host file and all the hosts will be updated. 
Another issue one might face during synchronization is that small adjustments in the JCLs / scripts are needed e.g. replacing the userid before an import. For this use case, all the files are converted from EBCDIC to ASCII upon export and vice versa during the import. This enables you to easily edit the JCLs on your computer with your favorite editor.
