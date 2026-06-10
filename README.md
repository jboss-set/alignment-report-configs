#  Maven Dependency Updater Configuration

These are configuration files for the [Maven Dependency Updater](https://github.com/jboss-set/maven-dependency-updater)
tool.

The tool is currently used to generate email reports about possible dependency upgrades for 
[Wildfly](https://github.com/wildfly/wildfly) and some other upstream projects.

## Configuration File Format

```json
{
  # List of repositories to scan for new artifact versions
  "repositories": {
    "MRRC GA": "https://maven.repository.redhat.com/ga/",
    "MRRC EA": "https://maven.repository.redhat.com/earlyaccess/all/"
  },

  # Do not report dependencies that are only defined in following scopes
  "ignoreScopes": ["test"],
  
  # Rules defining what we want to report for specific G:As
  # Only the rule most closely matching the dependency G:A is taken into account, e.g.:
  #
  # * "org.wildfly:wildfly-messaging" takes precedent over "org.wildfly:*",
  # * "org.wildfly:*" takes precedent over "org.wildfly.*:*".
  "rules": {

    # Artifact G:A
    "org.wildfly:wildfly-messaging": {

      # STREAM defines what kind of upgrades will be highlighted by bold font in the report.
      # By default newer MICRO versions are highlighted. The example bellow would make newer MINOR versions highlighted instead.
      # Valid options are MICRO / MINOR / MAJOR:
      "STREAM": "MINOR",
      
      # Only report versions prefixed with given string for this G:A:
      "PREFIX": "10.0.0",

      # Only report versions where the qualifier matches given regexp for this G:A:
      "QUALIFIER": "Beta\\d+"
      # alternatively provide a list of regexps:
      "QUALIFIER": [
        "Final",
        "SP\\d+"
      ]
    },

    # Artifact G:A can use wildcards, supported are "G:*", "G*:*", "*:*":
    "*:*": {
      "STREAM": "MICRO",
      "QUALIFIER": "Final"
    },

    # To disable artifacts from being reported, use `"G:A" : "NEVER"`:
    "org.jboss.*:*": "NEVER"
  }
}
```
