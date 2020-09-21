# How to create modular inputs using Splunk SDK for Python

[Modular inputs](http://docs.splunk.com/Documentation/Splunk/latest/AdvancedDev/ModInputsIntro) enable you to add new types of inputs to Splunk Enterprise, and have them treated as native Splunk Enterprise inputs. Your users interactively create and update your custom inputs using Splunk Web, just as they do for native inputs. If you're not already familiar with modular inputs, see the following topics to get started:

- Modular inputs overview
- Modular inputs basic example

Some examples of modular inputs include:

- [Real-time Windows performance monitoring](http://docs.splunk.com/Documentation/Splunk/latest/Data/Real-timeWindowsperformancemonitoring) : A perfmon input that runs a separate process for every input defined.
- [Splunk Add-on for Microsoft PowerShell](http://splunk-base.splunk.com/apps/82353/splunk-add-on-for-microsoft-powershell) : A modular input that runs PowerShell 3.0 scripts to collect data.
- [Twitter example](http://docs.splunk.com/Documentation/Splunk/latest/AdvancedDev/ModInputsExample#Twitter_example) : A modular input that streams JSON data from a Twitter source to Splunk Enterprise.
- [Amazon S3 example](http://docs.splunk.com/Documentation/Splunk/latest/AdvancedDev/ModInputsExample#S3_example) : A modular input that streams data from the Amazon S3 data storage service to Splunk Enterprise.

The Splunk SDK for Python enables you to use Python to create new modular inputs for Splunk Enterprise. This section of the Splunk SDK for Python documentation will show you how to create modular inputs using Python, and then how to integrate them with your Splunk Enterprise app.

This topic contains the following sections:

- Why use modular inputs?
- To create modular inputs programmatically
- Modular input SDK examples
- To write a modular input script in Python
- To add the modular input to Splunk Enterprise

## Why use modular inputs

Modular inputs are ideal for packaging and sharing technology-specific data sources. One of the primary reasons to use modular inputs is that it enables users to interact with key information using the familiar Splunk Manager interface, without needing to edit config files. Modular inputs also provide runtime controls and allow the input to specify per-event index-time settings.

More information about the differences between modular inputs and traditional scripted inputs is available here: [Modular inputs vs. scripted inputs](http://docs.splunk.com/Documentation/Splunk/latest/AdvancedDev/ModInputsIntro#Modular_inputs_vs._scripted_inputs).

## To create modular inputs programmatically

With the Splunk SDKs, you can create modular inputs programmatically using your preferred programming language, such as Python, C#, or Java. Here we describe how to create a modular input programmatically with Python.

To create a modular input, you first set up a modular input script. A modular input script does the following:

1. Return the introspection scheme to Splunk Enterprise. The introspection scheme defines the behavior and endpoints of the script. When Splunk Enterprise starts, it runs the script to determine the modular input's behavior and configuration.
2. Validate the script's configuration (optional). Whenever a user creates or edits an input, Splunk Enterprise can call the script to validate the configuration.
3. Stream data. The script streams event data that can be indexed by Splunk Enterprise. Splunk Enterprise invokes the script and waits for it to stream events.

The easiest way to create a modular input programmatically using Python is to inherit from the SDK's abstract base class, splunklib.modularinput.script. The preceding three steps are accomplished as follows using the Splunk SDK for Python:

1. Return the introspection scheme: Override the get_scheme method.
2. Validate the script's configuration (optional): Override the validate_input method. This is required if the scheme returned by get_scheme was set to use external validation (that is, it had Scheme.use_external_validation set to True).
3. Stream data: Override the stream_events method.

In addition, you must provide a simple main method that actually runs the script. It need only be one line.

Here's the skeleton of a new modular input script created in Python:

```python
import sys

from splunklib.modularinput import *

class MyScript(Script):

    def get_scheme(self):
        # Returns scheme.

    def validate_input(self, validation_definition):
        # Validates input.

    def stream_events(self, inputs, ew):
        # Splunk Enterprise calls the modular input,
        # streams XML describing the inputs to stdin,
        # and waits for XML on stdout describing events.

if __name__ == "__main__":
    sys.exit(MyScript().run(sys.argv))
```

## Modular input SDK examples

The Splunk SDK for Python includes two modular input examples in the /splunk-sdk-python/examples directory: random numbers and Github forks. To run these examples, you'll need to install them. First, run the setup script. Run the following command while in the root directory of the SDK: setup.py dist

The script will produce two .spl files in the /splunk-sdk-python/build directory: github_forks.spl and random_numbers.spl.

Next, install one or both of the .spl files within SplunkWeb: Once you're logged into Splunk, on the App menu, choose Manage apps, and then click the Install app from file button. In the dialog box that appears, navigate to the /splunk-sdk-python/build directory, and then choose the .spl file you want to install. After you've chosen the file, click the Upload button.

Once you've installed one or both of the example apps, their files are located here, where app_name represents the name of the app: $SPLUNK_HOME/etc/apps/app_name/

After the app is installed, the modular input you just enabled appears alongside other data inputs. From Splunk Manager (or, in Splunk Enterprise 6.0 or later, the Settings menu), click Data inputs, and find the name of the modular input you just added. Click Add new and fill in the settings that you specified when you created the modular input script. For instance, the Random Numbers example has fields for the name of the input and the minimum and maximum numbers to be produced. The Github Repository Forks example has fields for the name of the input and the owner and name of the repository.

To get a better understanding of how the examples work, take a look at the source code, which is located in the bin directory. Within bin is a well-commented Python script file (named using the name of the app) that does all the work for the modular input. Also within bin is the splunklib directory, which contains the same files as the SDK's splunklib directory.

## To write a modular input script in Python

Using the starting point from the To create modular inputs programmatically section, we'll now guide you through the creation of the components of a modular input script in Python. This is the same script that is located at /splunk-sdk-python/examples/random_numbers in the Splunk SDK for Python repository. It generates random numbers every half second to demonstrate event generation and streaming.

### The get_scheme method

When Splunk Enterprise starts, it looks for all the modular inputs defined by its configuration, and tries to run them with the argument --scheme. Splunkd expects each modular input to print a description of itself in XML to stdout. The SDK's modular input framework takes care of all the details of formatting the XML and printing it. You only need to override the get_scheme method and return a new Scheme object.

First, create a new Scheme object, providing both a name and description for it:

```python
    def get_scheme(self):
        scheme = Scheme("Random Numbers")
        scheme.description = "Streams events containing a random number."
```

In this case, Splunk Enterprise will display "Random Numbers" to users for this input, with the given description.

Next, specify whether you want to `use external validation` using the use_external_validation property. External validation is taken care of by overriding the [validate_input](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/modularinput.html#splunklib.modularinput.Script.validate_input) method. If you set external validation without overriding the validate_input method, the script will accept anything as valid.

```properties
        scheme.use_external_validation = True
```

If you set `use_single_instance` to True, the scheme will pass all the instances of the modular input to a single instance of the script. You're then responsible for handling all of the instances of the modular input.

```properties
        scheme.use_single_instance = True
```

Generally you only need external validation if there are relationships you must maintain among the parameters, such as requiring one variable to be less than another, or checking whether some resource is reachable or valid. If you don't choose external validation, Splunk Enterprise lets you specify a validation string for each argument and runs validation internally using that string.

In the example modular input, there are two variables, min and max, that represent the minimum and maximum values, respectively, for the generated random numbers. We'll add them to the scheme using the [Argument](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/modularinput.html#splunklib.modularinput.Argument) class and its properties:

```python
        min_argument = Argument("min")
        min_argument.data_type = Argument.data_type_number
        min_argument.description = "Minimum random number to be produced by this input."
        min_argument.required_on_create = True
        # If not using external validation, add something like:
        # min_argument.validation = "min > 0";
        scheme.add_argument(min_argument)

        max_argument = Argument("max")
        max_argument.data_type = Argument.data_type_number
        max_argument.description = "Maximum random number to be produced by this input."
        max_argument.required_on_create = True
        scheme.add_argument(max_argument)
```

After adding any validation variables to the scheme, return the scheme:

```python
        return scheme
```

### The validate_input method

The [validate_input](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/modularinput.html#splunklib.modularinput.Script.validate_input) method is where the configuration of an input is validated, and is only needed if you've set your modular input to use external validation. If `validate_input` does not throw an exception, the input is assumed to be valid. Otherwise it prints the exception as an error message when it tells splunkd that the configuration is not valid.

When you use external validation, after splunkd calls the modular input with the `--scheme` argument to get a scheme, it calls it again with the `--validate-arguments` option for each instance of the modular input in its configuration files, feeding XML on stdin to the modular input to get it to do validation. It calls it the same way again whenever a modular input's configuration is changed.

In our example, we're using external validation, since we want the `max` variable to always be greater than the `min` value. Our `validate_input` method contains basic logic that retrieves the two variables and then compares them to each other:

```python
    def validate_input(self, validation_definition):
        minimum = float(validation_definition.parameters["min"])
        maximum = float(validation_definition.parameters["max"])

        if minimum >= maximum:
            raise ValueError("min must be less than max; found min=%f, max=%f" % minimum, maximum)
```

### The stream_events method

The [stream_events](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/modularinput.html#splunklib.modularinput.Script.stream_events) method is where the event streaming happens. Events are streamed into stdout using an InputDefinition object as input that determines what events are streamed. In the case of the Random Numbers example, for each input, the values are first retrieved and cast as floats. Then, an [Event](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/modularinput.html#splunklib.modularinput.Event) object is created, its data fields are set, and then it's written using the [EventWriter](http://docs.splunk.com/DocumentationStatic/PythonSDK/1.6.5/modularinput.html#splunklib.modularinput.EventWriter).

```python
    def stream_events(self, inputs, ew):
        for input_name, input_item in inputs.inputs.iteritems():
            minimum = float(input_item["min"])
            maximum = float(input_item["max"])

            event = Event()
            event.stanza = input_name
            event.data = "number=\"%s\"" % str(random.uniform(minimum, maximum))

            ew.write_event(event)
```

### Optional: Set up logging

It's best practice for your modular input script to log diagnostic data to splunkd.log. Use an EventWriter's log method to write log messages, which include both a standard splunkd.log level (such as "DEBUG" or "ERROR") and a descriptive message.

## To add the modular input to Splunk Enterprise

With your modular input script completed, you're ready to integrate it into Splunk Enterprise. The basic steps are:

1. Package the script along with the contents of the SDK library.
2. Create an app.conf file.
3. Create an inputs.conf.spec file.
4. Move the modular input package into the $SPLUNK_HOME/etc/apps/ directory.

### Package the script and the SDK library

To add a modular input that you've created in Python to Splunk Enterprise, you'll need to first add the script as a Splunk Enterprise app.

1. Create a directory that corresponds to the name of your modular input script—for instance, "random_numbers"—in a location such as your Documents directory. (You'll copy the directory over to your Splunk Enterprise directory at the end of this process.)
2. Within the app directory, create the following three empty directories:
    - bin
    - default
    - README
3. From the root level of the Splunk SDK for Python, copy the splunklib directory into the bin directory you just created.
4. Copy the modular input Python script (for instance, random_numbers.py) into the bin directory.

Your app directory structure now looks like this:

```txt
.../
  bin/
    app_name.py
    splunklib/
      __init__.py
      ...
  default/
  README/
```

### Create an app.conf file

Within the default directory, create a file called [app.conf](http://docs.splunk.com/Documentation/Splunk/latest/Admin/Appconf). This file is used to maintain the state of an app or customize certain aspects of it in Splunk Enterprise. The contents of the `app.conf` file can be very simple:

```properties
[install]
is_configured = 0

[ui]
is_visible = 1
label = My App

[launcher]
author = Splunk Inc
description = My app is awesome.
version = 1.0
```

For more examples of what to put in the app.conf file, see the corresponding files in the modular inputs examples.

### Create an inputs.conf.spec file

You need to define the configuration for your modular input by editing the inputs.conf.spec file manually. See "[Create a modular input spec file](http://docs.splunk.com/Documentation/Splunk/latest/AdvancedDev/ModInputsSpec#Create_a_modular_input_spec_file)" in the main Splunk Enterprise documentation for instructions, or take a look at the SDK samples' inputs.conf.spec file, which is in the application's README directory. For instance, the following is the contents of the Random Numbers example's inputs.conf.spec file:

```properties
[random_numbers://<name>]
*Generates events containing a random floating point number.

min = <value>
max = <value>
```

Move the modular input script into your Splunk Enterprise install
Your directory structure now looks like this:

```txt
.../
  bin/
    app_name.py
    splunklib/
      __init__.py
      ...
  default/
    app.conf
  README/
    inputs.conf.spec
```

The final step to install the modular input is to copy the app directory to the following path: `$SPLUNK_HOME/etc/apps/`.

Restart Splunk Enterprise, and on the App menu, click Manage apps. If you wrote your modular input script correctly, the name of the modular input—for instance, "Random Numbers"—will appear here. If not, go back and double-check your script.

If your modular input appears in the list of apps, in Splunk Manager (or, in Splunk Enterprise 6.0 or later, the Settings menu), under Data, click Data inputs. Your modular input will also be listed here. Click Add new, fill in any settings your modular input requires, and click Save.

You've now configured an instance of your modular input as a Splunk Enterprise input.