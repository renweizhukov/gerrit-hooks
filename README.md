# gerrit-hooks
All gerrit-related hooks.

## patchset-created

Courtesy of https://github.com/tru/redmine-gerrit-scripts/blob/master/README.md.

This is a simple Gerrit hook for updating Redmine with information about what changes are in Gerrit review.

It parses the commit messages from Gerrit and looks for a Redmine issue in the style of "#1028".

It then adds a informative message to the Redmine issue about the review URL and other data. It can also change the status of the issue to indicate that this issue is now under gerrit review.

Script is tested with Redmine 3.2.0.stable and Gerrit 2.11.4.

### Installation

1. Edit the script to set the variables according to your Gerrit and Redmine server setup.
(1) REDMINE_API_KEY
(2) REDMINE_HOST
(3) REDMINE_HOST_PORT_NUMBER
(4) REDMINE_HOST_USING_SSL
(5) GERRIT_PROJECTS

2. Put the script in the $GERRIT_HOME/hooks directory. If gerrit is installed on a Linux server, don't forget to make the script executable.

```
chmod +x $GERRIT_HOME/hooks/patchset-created
```

3. If the name of the script is changed from patchset-created to something else don't forget to update the [Hooks] section of the $GERRIT_HOME/etc/gerrit.config to include patchsetCreatedHook = new_script_name. For more information see the gerrit hooks documentation 
at https://gerrit-review.googlesource.com/Documentation/config-hooks.html#_configuration_settings.

```
[hooks]
    patchsetCreatedHook = new_script_name
```

## ref-update

This is a simple Gerrit server hook for checking the message format of the commits 
being pushed to Gerrit. If any commit message doesn't meet one of the three requirements
below, the push will be rejected:
(1) The commit message should conform to the following template:
####################################################
<commit message title>  

This commit fixes #123, refs #456, and refs #789. 

<code change short description>

HOW BUILT
<how did you build this code change>

HOW VERIFIED
<how did you verify this code change>

Change-Id: I214325432654654adf324325432ec32435435456
Signed-off-by: John Doe <johndoe@company.com>
####################################################
Note that the Change-Id and Signed-off-by are generated by git commit-msg hook and git commit option '-s', respectively.
(2) The length of the commit message title should not be greater than 100 characters.
(3) The fixed/referenced issue ID(s) should be mentioned in the second paragraph.

Script is tested with Gerrit 2.11.4.

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

3. If the name of the script is changed from ref-update to something else don't forget to update the [Hooks] section of the $GERRIT_HOME/etc/gerrit.config to include refUpdateHook = new_script_name. For more information see the gerrit hooks documentation at https://gerrit-review.googlesource.com/Documentation/config-hooks.html#_configuration_settings.

```
[hooks]
    refUpdateHook = new_script_name
```

## Gerrit Logging

All print statements are appended to the $GERRIT_HOME/logs/error_log. 
