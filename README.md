# gerrit-hooks
All gerrit-related hooks.

## patchset-created

This is a simple Gerrit hook for updating Redmine with information about what changes are in Gerrit review.

It parses the commit messages from Gerrit and looks for a Redmine issue in the style of "#1028".

It then adds a informative message to the Redmine issue about the review URL and other data. It can also change the status of the issue to indicate that this issue is now under gerrit review.

Script is tested with Redmine 3.2.0.stable and Gerrit 2.11.4.

### Installation

1. Edit the script to set the variables according to your Gerrit and Redmine server setup.
(1) REDMINE_API_KEY
(2) REDMINE_HOST
(3) REDMINE_ISSUE_ID_REGEX
(4) GERRIT_URL_REGEX
(5) GERRIT_PROJECTS

2. Put the script in the $GERRIT_HOME/hooks directory. If gerrit is installed on a Linux server, don't forget to make the script executable.

```
chmod +x $GERRIT_HOME/hooks/patchset-created
```

3. Install the python-redmine package.

```bash
$ pip install python-redmine
```

If you receive the error "Permission Denied", please use the sudo command.

```bash
$ sudo -H pip install python-redmine
```

4. If the name of the script is changed from patchset-created to something else don't forget to update the [Hooks] section of the $GERRIT_HOME/etc/gerrit.config to include patchsetCreatedHook = new_script_name. For more information see the gerrit hooks documentation 
at https://gerrit-review.googlesource.com/Documentation/config-hooks.html#_configuration_settings.

```
[hooks]
    patchsetCreatedHook = new_script_name
```

## ref-update

This is a simple Gerrit server hook for checking the commit messages being pushed to Gerrit. 
If the commit is a revert one, then skip the commit message check; otherwise perform the 
commit message check. Specifically, if any commit message doesn't meet one of the four requirements 
below, the push will be rejected:
(1) The commit message should conform to the following template:
####################################################
&lt;commit message title&gt;   

This commit fixes/refs #123, refs #456, and refs #789.
&lt; FOR INFORMATION ONLY! PLEASE DELETE THIS!
Note that if you claims that this commit fixes an issue, submitting this commit will resolve the issue as Fixed. 
Thus if you plan to fix one issue with more than one commit, please only use the keyword "fixes" for the last commit.
FOR INFORMATION ONLY! PLEASE DELETE THIS!&gt; 

&lt;code change short description&gt;

HOW BUILT

&lt;how did you build this code change&gt;

HOW VERIFIED

&lt;how did you verify this code change&gt;

Change-Id: I214325432654654adf324325432ec32435435456
Signed-off-by: John Doe &lt;johndoe@company.com&gt;
####################################################
Note that the Change-Id and Signed-off-by are generated by git commit-msg hook and git commit option '-s', respectively.
(2) The length of the commit message title should not be greater than 100 characters.
(3) The non-zero fixed/referenced issue ID(s) should be mentioned in the second paragraph.
(4) The fixed/referenced issue ID(s) should belong to the redmine project linked to the gerrit project.

Script has been tested with Gerrit 2.11.4 and 2.12.2.

### Installation

1. Edit the script to set the variables according to your Gerrit server setup.
(1) COMMIT_MSG_TEMPLATE_REGEX
(2) COMMIT_MSG_ISSUE_REGEX
(3) MAX_MSG_TITLE
(4) GERRIT_PROJECTS

2. Put the script in the $GERRIT_HOME/hooks directory. If gerrit is installed on a Linux server, don't forget to make the script executable.

```
chmod +x $GERRIT_HOME/hooks/ref-update
```

3. Install the python-redmine package.

```bash
$ pip install python-redmine
```

If you receive the error "Permission Denied", please use the sudo command.

```bash
$ sudo -H pip install python-redmine
```

4. If the name of the script is changed from ref-update to something else don't forget to update the [Hooks] section of the $GERRIT_HOME/etc/gerrit.config to include refUpdateHook = new_script_name. For more information see the gerrit hooks documentation at https://gerrit-review.googlesource.com/Documentation/config-hooks.html#_configuration_settings.

```
[hooks]
    refUpdateHook = new_script_name
```

## Error Diagnostics
If you edit the script on Windows and use it on Linux, you may encounter some mysterious errors like "cannot run program hook". To fix this error, you just need to 
change the EOL characters from CRLF into LF.

## Gerrit Logging

All print statements are appended to the $GERRIT_HOME/logs/error_log. 
