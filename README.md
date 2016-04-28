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

TODO

## Gerrit Logging

All print statements are appended to the $GERRIT_HOME/logs/error_log. 
