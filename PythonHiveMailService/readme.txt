Pre Section
Summary
This python file searches a desired mailbox, and creates alert/cases automatically while scanning for observables and attatched files.
These main repositories are used.
TheHive4Py
https://github.com/TheHive-Project/TheHive4py
This repository provides a Python API that can be used by anyone to do tasks such as create cases or updating cases using
simple python functions.
imap2thehive
https://github.com/xme/dockers/tree/master/imap2thehive
This repository is the bulk of the python script which crawls a mailbox and creates cases to a defined hive server.

Section 1
How to Run the file
--> Create a scheduler or job to run the python file a set amount of minutes, this will scan
the desired inbox and create cases/alerts
--> You can also run the file using any python3+ interpreter for a manual test if wanted.

Section 2
How to configure
[imap] -> The main inbox the script will crawl through, requires a host(email server name),
port(port of the email server), user(username),password(password),folder name and an expunge 
variable. The expunge defines whether it will move it out of the box or not, set is as true
to automatically do this, set it to false to leave the email in the same box after read.
[imap2] -> By default, this is disabled. This was custom built. This allows two email bozes to be 
searched, to enable to this feature, you will need to edit the python file and comment out
the "imap2" section.
[thehive] -> This is the server information for the hive, the url, username of a hive admin account,
password of a hive account are all needed. Oberservables defines if observables will be
captured or not. Whitelist allows observerables to whitelisted if occurs often. Observables is true/false,
whitelists is set to a whitelists file for example, imap2thehive.whitelists.
[thehive2] -> Similar to imap2, by default, this is disabled. To enable it, please comment out
thehive2 config commented section in the python file to enable it.
[alert] -> defines the default TLP and tags for an alert creation, tlp is a number, 0-4, tags is a comma
seperatted list. You can read the full documentation on this here:
https://media.readthedocs.org/pdf/thehive-doc/develop/thehive-doc.pdf
[case] -> defines the default TLP, tags,tasks for a case creation. [template] defines the default template if one 
cant be found in [templates]. [templates] is a list that searches the string in the subject header of the email and
creates a case before the hyphen. For example, testTemplate-"critical incident." if "critical incident" is in the subject
email header, it uses testTemplate. [files] defines what files can be read. xlsx files can also be read in the python file. See the #ONLY WORKS FOR .CSV comment in the python file.

Section 3
Updating a case
->A case can be updated via email by having the word "update" in the header and having a body that follows this format:

---Updated Information
Case ID: 95 
Resolved: Yes
Resolution Status: Yes
Impact Status: No
Summary: Resolved
Tags: best,best2,best3
Title: Hello World!
TLP: 1
Albert ID: 30
Severity: 3
ReplaceTags: No
---

Case ID is REQUIRED, otherwise the case cannot be found to be updated.

Section 4
Observables
Observables can be added creating custom regexs in the observerableTypes dictionary in the python file. Observables are keywords attatched to an alert or case that can be found in the body or
attatchments in the email. To add your own, simply add a new entry to this dictionary with your regex to find that observable. For example:
{'type': 'Severity', 'regex': r'\b(Severe|Normal|Strange)\b'}. Type defines the name of the observable, Severity in this case. The regex pattern is then follow after 'regex'. If you need help making your own regexes,
please visit the following link:
https://regexr.com/

The Albert ID is customly parsed from the body and is coded into the python script. It is found by the following regex: 'Albert Incident #: (\d+)'. If the following regex is not used, the albert ID will not be added to the customField of the case/alert.


AlertMailingService-
This folder contains a python file that automatically scans the email box and creates alert instead
of the normal feature of creating cases. This means configuring templates in the config has no real effect.
CaseMailingService-
This folder contains a python file that creates cases automatically, therefore most of the configuration applies.