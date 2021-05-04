# Python3 Migration

## Python3 migration with the Splunk platform

On January 1, 2020, Python version 2.x was officially deprecated by the Python Software Foundation. Python packages and tools have ended or are ending support for Python 2, and new Python packages don't support Python 2. This does not impair Splunk's ability to provide support for Splunk Cloud and Splunk Enterprise, including Python 2.7. However, to continue to leverage community support and maintain compatibility with the many third party projects that use Python, Splunk has migrated Splunk Cloud and Splunk Enterprise, supported Premium Solutions, and supported Splunkbase apps and add-ons to be compatible with Python 3.7.



This manual helps identify prerequisites, required changes and steps for migrating impacted Splunk products and apps to Python 3. As Splunk makes new tools and advice available, this content will be updated. When this content is revised, changes updates will be tracked in Splunk Answers.

Splunk has released Splunk Cloud and Splunk Enterprise versions 8.x to support the migration from Python 2 to Python 3. Splunk has migrated impacted features to Python 3.7, and versions 8.x include both the Python 2.7 and Python 3.7 runtimes, to help customers and developers transition Python in apps from Python 2 to Python 3. Splunk will remove the Python 2.7 runtime altogether in a future release.

### Splunk administrators

Administrators of Splunk Cloud and Splunk Enterprise deployments might need to take steps to prepare for Python 3.7 in version 8.x, as there are prerequisites to this upgrade. See details in Changes to Splunk Enterprise.

Splunk provides the Splunk Platform Upgrade Readiness app for admins to scan deployed apps for any components impacted by migration to Python 3. For more information, see the Splunk Platform Upgrade Readiness App.

### Splunk app developers

Splunk app and add-on developers must revise apps and add-ons that use Python scripts. Revised Splunkbase apps will need to be resubmitted after validation with version 8.x. For details about specific script changes required to support version 8.x, see Python Development in Splunk Enterprise. Custom apps for Splunk Cloud will follow the same guidelines as custom apps for Splunk Enterprise, with the added requirement of passing Cloud vetting.

Splunk provides the Splunk Platform Upgrade Readiness app for developers to scan deployed apps for any components impacted by migration to Python 3. For more information, see the Splunk Platform Upgrade Readiness App. Developers should also review all issues covered in this manual and test app and add-on revisions with the Splunk Enterprise version 8.x before resubmitting to Splunkbase.

### Splunk Cloud customers

Beginning with version 8.0, Splunk Cloud added Python 3 support and updated the out-of-the-box Splunk Cloud capabilities while continuing to provide Python 2 support for existing apps that have not yet been upgraded to Python 3. A future version of Splunk Cloud will remove support for Python 2. Whether you are currently on Splunk Cloud 7.x or 8.x, any apps or add-ons in your Splunk Cloud instances that are not Python 3 compatible must be upgraded or removed. Splunk Cloud customers should take the following steps:

- Update your private apps to be compatible with Splunk Cloud 8.x and Python 3. You should follow the same process as Splunk app developers to update private apps. However, for Splunk Cloud, instead of uploading the app to Splunkbase, upload your private app to Splunk Cloud.
- Update apps you have installed on your Splunk Cloud deployment from Splunkbase to versions that are compatible with both your current version of Splunk Cloud and Splunk Cloud 8.x. Decide whether to update remaining incompatible apps or to find another solution. Use the Splunk Cloud Monitoring Console, to identify which apps on your Splunk Cloud deployment must be updated.

This Python 3 compatibility requirement applies to all public and private apps in your Splunk Cloud environment. Splunk will continue to work closely with third-party Splunkbase developers to provide updated Python 3-compatible versions of their published apps. You must install the new versions of those Splunkbase apps and upgrade all private apps to ensure Python 3 compatibility.

For more information, see Splunk Cloud.

### Splunk solutions customers

Customers of some Splunk solutions must update ML models to support Python 3, as specified in Splunk IT Service Intelligence and Splunk Machine Learning Toolkit.

This information is subject to change at any time, at the sole discretion of Splunk Inc. and without notice. This roadmap information shall not be incorporated into any contract or other commitment. Splunk undertakes no obligation to either develop or deliver any product, features, or functionality described here.

## Changes to Splunk Enterprise

The following changes to Splunk Enterprise cause breaking changes to existing Python scripts:

- Python scripts in the deprecated module system. Any scripts with Python 2 syntax in [app]>appserver>modules>[module name] that aren't Python 3 compatible will cause UI errors, and should be made dual compatible with both Python 2 and 3.
- Custom web controllers (such as CherryPy endpoints). Requires script-level compatibility with Python 3.7. Failure to make scripts compatible with Python 3.7 may cause issues starting Splunk Web.
- Custom Mako templates. Requires script-level compatibility with Python 3.7. Failure to make scripts compatible with Python 3.7 may cause issues starting Splunk Web.
- Advanced XML (deprecated in Splunk version 6.3): removed. If possible, replace Advanced XML with Simple XML. For more information about alternatives to Advanced XML available in Splunk Enterprise, see Building customizations for the Splunk platform.
- Splunk Web Legacy Mode (deprecated in Splunk version 6.4): removed. Do not set appServerPorts = 0 in web.conf.

To prevent issues starting Splunk Web, revise apps for Python 3 compatibility. If an app cannot be upgraded, it must be removed for Splunk Web to start.

In addition to breaking changes and upgrade steps, a number of features and components are impacted with Splunk Enterprise version 8.x, but deployments can be migrated to Python 3.7 over time. Splunk Enterprise version 8.x includes both Python 2.7 and Python 3.7 interpreters, with Python 2.7 enabled for all custom Python scripts outside of those shipped with Splunk Enterprise.

The Python 2.7 interpreter is still used by default in Splunk Enterprise version 8.x for features that include:

- Custom search commands
- Custom REST endpoints
- Scripted authentication
- Scripted inputs
- Modular inputs
- Scripted lookups
- Custom alert actions
- Modular alerts
- Cold-to-frozen scripts

Note that scripted alerts were deprecated in version 6.3 and are not supported in Python 3.7. Convert any scripted alerts to custom alert actions in order to adopt Python 3.7.

Using Python 3-only syntax for these features might be incompatible with the Python 2.7 interpreter which will be enabled by default in the Splunk Enterprise version 8.x. Python scripts that are compatible with both Python 2 and Python 3 should work with either Python interpreter used by Splunk Enterprise, and is recommended for developers. For more information about making Python scripts dual-compatible, see Python Development in Splunk Enterprise.

### Splunk Web

Unlike Splunk Enterprise version 8.x, Splunk Web supports only Python version 3.7. Any scripts that depend on Splunk Web must be upgraded to use syntax compatible with both Python 2.7 and 3.7. This will allow upgrades to 8.x from 7.x versions of Splunk Enterprise.

To prevent issues starting Splunk Web, revise apps for Python 3 compatibility. If an app cannot be upgraded, it must be removed for Splunk Web to start.

### Python interpreter settings

Splunk Enterprise version 8.x includes a global setting, python.version, to specify which Python interpreter to use across an instance. The global setting resides in the server.conf file, located in $SPLUNK_HOME/etc/system/local/. The stanza that controls Python version is [general]. For more information how Splunk Enterprise uses configuration files, see About configuration files.

>> For Splunk Enterprise version 8.1, python.version defaults to python3 within server.conf. For prior versions of Splunk Enterprise, python.version defaults to python2.

To force Splunk Enterprise to use only the Python 3 interpreter regardless of script-level setting, set python.version = force_python3. Use this setting if you cannot run Python 2.7 past its EOL date of January 1, 2020, or if your Splunk Enterprise deployment and all Splunk apps and add-ons are fully migrated and ready to run Python 3 only.

Splunk Enterprise also includes python.version settings to control which version of the Python interpreter is used by Splunk Enterprise at the script-level. For the following scripts, the python.version setting resides in the corresponding conf file:


Script type	File
Custom search commands	commands.conf
Modular inputs	inputs.conf
Scripted inputs	inputs.conf
Custom alert actions	alert_actions.conf
Scripted lookups	transforms.conf
Custom REST endpoints	restmap.conf
Scripted authentication	authentication.conf
coldToFrozenScript	indexes.conf
By default, the script-level setting of python.version is not set, and the script will use the Python interpreter specified by the global setting in server.conf. Setting python.version to default or python also uses the Python interpreter specified by the global setting in server.conf. If set to python2 or python3, the corresponding Python interpreter will be used. This overrides the global setting, except if the global setting is force3, in which case Python 3 is always used.

Set python.version to python3 or default to remove Python 3 migration-related start up warnings for your impacted apps.

Apps that must be written in Python 3-only syntax should set python.version to python3 in the appropriate .conf files for individual scripts. Developers should not set python.version in server.conf. For dual-compatibility with both Python 2 and 3, set python.version to python3 in the following .conf files:

commands.conf
inputs.conf
restmap.conf (for custom endpoints)
transforms.conf (for scripted lookups)
Additional required setting of python.version specific to your app can be reported by running AppInspect. For more information, see the Splunk AppInspect tool.

Setting python.version for coldToFrozenScript applies if the canonical path to the Python interpreter is used. However, note that for coldToFrozen:

    * scripts set executable on UNIX with a #! shebang line pointing to a valid interpreter.
If your script is specified with #! /usr/bin/env python, then `python.version` will be ignored for coldToFrozen. Also note that for warmToCold, this is always how the Python script is specified, so there is no applicable python.version for warmToCold.

Search and Reporting
If you have modified Splunk Search and Reporting with scripts or other customizations using Python 2, you must update these scripts to use Python 3 syntax or to be dual-compatible with both Python 2 and Python 3.

If you must maintain Python 2 compatibility, use Python compatibility libraries provided with Splunk Enterprise to help make apps and add-ons compatible with both Python 2 and Python 3. (Six, Python-future, 2to3). Splunk also updated Splunk Enterprise 7.x maintenance releases to include these Python compatibility libraries. These Splunk-provided libraries should not be used for any other apps; all custom or Splunkbase apps should package their own libraries to respect app structures.

Analytics for Hadoop and Hadoop Data Roll
Analytics for Hadoop and Hadoop Data roll do not support Python 3 in Splunk Enterprise version 8.x. When using Hadoop with Splunk Enterprise:

Do not set set python.version = python3 for the global python.version setting, which resides in the server.conf file.
Do not remove the Python 2.7 runtime. If your deployment requires the removal of Python 2.7 for for security compliance reasons, contact Splunk Support.
Splunk Platform Upgrade Readiness app
Splunk provides the Splunk Platform Upgrade Readiness app for admins to scan deployed apps for any components impacted by migration to Python 3. The app is recommended to prepare for an upgrade to Splunk Enterprise version 8.x. For more information, see the Splunk Platform Upgrade Readiness App.

Splunkbase apps and add-ons
Impacted Splunkbase apps and add ons must be resubmitted to Splunkbase after validation of compatibility with the Splunk Enterprise version 8.x, including Python 3 testing with AppInspect. Apps that are marked compatible with Splunk Enterprise 7.x and below are Python 2.7-compatible only, while apps that are marked compatible with Splunk Enterprise 8.x are Python 3.7-compatible only. Apps that are marked compatible with Splunk Enterprise 7.x and 8.x are compatible with both Python 2.7 and 3.7.


Python development with Splunk Enterprise
Check this manual often for updated information about the Splunk platform Python 3 migration. The content is subject to change.

The migration to Python 3 impacts Python scripts developed by Splunk app and add on developers and admins. In addition to changes to Python scripts, there are additional settings for Splunk administrators and prerequisites for upgrading to Splunk Enterprise version 8.x. For more information, see Changes to Splunk Enterprise. For Splunk Enterprise version 8.x upgrade instructions, see Choose your Splunk Enterprise upgrade path for the Python 3 migration.

Developers must update Python scripts used in apps and add-ons for compatibility with Splunk Enterprise version 8.x. For guidelines for updating Python scripts in general, see Python Code Compatibility.

The following Splunk Enterprise features will require script-level compatibility with Python 3.7:

Custom web controllers (such as CherryPy endpoints)
Custom Mako templates
These must be made dual-compatible with both Python 2 and 3 to prevent breakage for customers upon upgrade.

Splunk Enterprise provides settings to specify which Python interpreter to use at global and script levels, covered in Changes to Splunk Enterprise.

Removal of deprecated Splunk platform features
Some deprecated features will be removed from the Splunk Enterprise Python 3 release:

Advanced XML (deprecated in Splunk version 6.3). If possible, replace Advanced XML with Simple XML. For more information about alternatives to Advanced XML available in Splunk Enterprise, see Building customizations for the Splunk platform.
Splunk Web Legacy Mode (deprecated in Splunk version 6.4): do not set appServerPorts = 0 in web.conf.
Writing scripts compatible with Python 2 and Python 3
Developers must make all Python files and scripts compatible with Python 3 to be compatible with Splunk Enterprise version 8.x. When making apps and scripts compatible with Python 3, Splunk recommends writing dual-compatible Python code that works with both Python 2 and Python 3 interpreters. For more information about Python compatibility libraries, see Python Code Compatibility.

Apps that must be written in Python 3-only syntax should set python.version to python3 in the appropriate .conf files for individual scripts. Developers should not set python.version in server.conf. For more information about python.version settings, including when to set it for dual-compatibility with both Python 2 and 3, see Changes to Splunk Enterprise.

You should properly store and import cross-compatible Python libraries and update the Python path according to guidelines provided in The directory structure of a Splunk App in Splunk developer docs.

Running against earlier indexer tiers
For apps that might run against a Splunk Enterprise version 7.3.x or earlier indexer tier, admins should ensure those apps use dual-compatible Python syntax. This is because custom search commands and scripted lookups will be passed to the indexer tier as part of the knowledge bundle, and any Python 3-specific syntax will fail on the indexer.

Module naming conflicts
You should also rename any files that conflict with Python standard modules or Splunk libraries, such as files named test.py or html.py. Use different, non-reserved names to avoid namespace conflicts in Python 3.

Splunk SDK for Python
The Splunk SDK for Python API and service wrappers are dual-compatible with Python 2 and Python 3, starting with version 1.6.5. Upgrade to the latest version of the Splunk SDK for Python to help make scripts that use the Splunk SDK for Python compatible with the Splunk Enterprise version 8.x.

Identifying Python scripts
Splunk provides the Splunk Platform Upgrade Readiness app for admins to scan deployed apps for any components impacted by migration to Python 3. For more information, see the Splunk Platform Upgrade Readiness App.

You can also manually identify possibly impacted Python scripts in your app or deployment by taking the following steps:

Identify files ending in *.py.
Identify files in $SPLUNK_HOME/etc/apps/$<app_name>/bin/. These are typically custom scripts or inputs, which might not necessarily end in *.py. but can still be implicitly executed by the Python interpreter used by Splunk Enterprise.
Identify any other files explicitly executed by the Python interpreter. These files are often executed by the command splunk cmd python $<script_name>.py. These files could contain shell scripts or could exist outside an app's or deployment's standard directories.
Splunk Web
Unlike Splunk Enterprise version 8.x, Splunk Web supports only Python version 3.7. Any scripts that depend on Splunk Web must be upgraded to use syntax compatible with both Python 2.7 and 3.7. This will allow upgrades to 8.x from 7.x versions of Splunk Enterprise.

Testing your app
For apps with scripts cross-compatible with both versions of Python, you will need to test your application in at least two Splunk Enterprise test deployments:

1. Use a Splunk Enterprise 7 deployment (version 7.2 or later) to test that your app runs as expected with a Python 2 runtime. Splunk Enterprise 7.2 or later forces your entire application to run in Python 2. 2. Use a Splunk Enterprise 8.x deployment with specific configuration settings to force your application to run in Python 3. You have two options:

Set python.version=python3 in server.conf's [general] stanza to force all scripts in all applications to run in Python 3 only.
Set python.version=python3 in the appropriate stanza of every .conf file that specifies Python scripts in your application.
For more information, see Python interpreter settings.

If you are creating an app that only runs in Python 3, you will only need a test environment for Splunk Enterprise 8.x.

If your app uses Python that runs in the appserver, such as in the module system, you will not be able to select which Python runtime will be used for these scripts. Splunk Enterprise versions previous to Splunk Enterprise 8.x will always attempt to run these scripts with Python 2, and Splunk Enterprise 8.x will always attempt to run these scripts with Python 3. Because of this, any app that you upload to Splunkbase and flag as 8.x compatible must be Python 3 compatible.

