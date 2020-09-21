# Getting Started

## Getting Started with the Splunk SDK for Python

So you've met the Splunk® SDK for Python, now what?

### Get it

The first thing you have to do is [get the SDK](http://dev.splunk.com/view/python-sdk/SP-CAAAEFB), get Splunk, and any other requirements. Then, install the SDK. That's it.

From here on out, we're assuming you know a little about using Splunk already, have some data indexed, and maybe saved a search or two. But if you're not there yet and need some more Splunk education, we have you covered:

- If you want a deeper description of Splunk's features, see the Splunk documentation.
- Try the [Tutorial](http://docs.splunk.com/Documentation/Splunk/latest/SearchTutorial/WelcometotheSearchTutorial) in the Splunk documentation for a step-by-step walkthrough of using Splunk Web with some sample data.
- Remember, the Splunk SDKs are built as a layer over the Splunk REST API. While you don't need to know the REST API to use this SDK, you might find it useful to read the [Splunk REST API basics](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTusing) or browse the [Splunk REST API Reference](http://docs.splunk.com/Documentation/Splunk/latest/RESTREF/RESTcontents).

### Poke it

Find out what makes the SDK tick—try it out, [play with the examples](http://dev.splunk.com/view/python-sdk/SP-CAAAEFK), and [run the unit tests](http://dev.splunk.com/view/python-sdk/SP-CAAAEDH).

You can make things easier by saving your login credentials in the [.splunkrc file](http://dev.splunk.com/view/python-sdk/SP-CAAAEFC) so you don't have to enter your login info each time you run an example. It's up to you.

### Code it

When you're ready to get your hands dirty, check out the [Python code examples](https://github.com/splunk/splunk-sdk-python/tree/master/examples) on GitHub for inspiration.

## Requirements for Splunk SDK for Python

Here's what you need to get going with the Splunk SDK for Python:

- [Splunk Enterprise](http://dev.splunk.com/view/python-sdk/SP-CAAAEFB#splunk)
- [Python](http://dev.splunk.com/view/python-sdk/SP-CAAAEFB#python)
- [Splunk SDK for Python](http://dev.splunk.com/view/python-sdk/SP-CAAAEFB#sdk)

### Splunk Enterprise

If you haven't already installed Splunk Enterprise, download it here. For more information about installing and running Splunk Enterprise and system requirements, see the Splunk Enterprise Installation Manual.

### Python

The Splunk SDK for Python supports Python versions 2.7 and 3.x.

### Splunk SDK for Python

[Download the Splunk SDK for Python as a ZIP](http://dev.splunk.com/goto/sdk-python) and extract the files.
If you want to verify your download, download an MD5 or download a SHA-512.
To contribute to the Splunk SDK for Python, see [the Splunk SDK for Python on GitHub](https://github.com/splunk/splunk-sdk-python).

## Install Splunk SDK for Python

Install the Splunk® SDK for Python libraries by doing the following:

1. Use Easy Install:
    ```bash
    [sudo] easy_install splunk-sdk
    ```
    Or, use pip:
    ```bash
    [sudo] pip install splunk-sdk
    ```
    Or, run setup.py on the resources you cloned from GitHub or downloaded from the Splunk dev site:
    ```bash
    [sudo] python setup.py install
    ```
2. Add the root path of the Splunk SDK for Python to the PYTHONPATH environment variable (this is a requirement for running the examples and individual unit tests). For example, add the following line to your ~/.bash_profile:
    ```bash
    export PYTHONPATH=~/splunk-sdk-python
    ```

You can also install the Python egg from pip:

```bash
[sudo] pip install --egg splunk-sdk
```

### Troubleshooting tip

If you get this type of error when you try to run your first program:

```python
From FOO import error, parse
ImportError: No module named FOO
```

It probably means you still need to add the root directory of the Splunk SDK for Python to your PYTHONPATH.

## Splunk SDK for Python Utilities

This topic describes an optional utility you can use with the Splunk SDK for Python.

### Save login credentials for examples and unit tests

To connect to Splunk, all of the SDK examples and unit tests take command-line arguments that specify values for the host, port, and login credentials for Splunk. For convenience during development, you can store these arguments as key-value pairs in a text file named .splunkrc. Then, when you don't specify these arguments at the command line, the SDK examples and unit tests use the values from the .splunkrc file.

To set up a .splunkrc file:

1. Create a text file with the following format:
    ```properties
    # Splunk host (default: localhost)
    host=localhost
    # Splunk admin port (default: 8089)
    port=8089
    # Splunk username
    username=admin
    # Splunk password
    password=yourpassword
    # Access scheme (default: https)
    scheme=https
    # Your version of Splunk (default: 5.0)
    version=6.4.2
    ```
2. Save the file as .splunkrc in the current user's home directory.
    __On Windows__
    Save the file as:
    ```bash
    C:\Users\currentusername\.splunkrc
    ```
    You might get errors in Windows when you try to name the file because .splunkrc looks like a nameless file with an extension. You can use the command line to create this file—go to the C:\Users\currentusername directory and enter the following command:
    ```bash
    Notepad.exe .splunkrc
    ```
    Click Yes, then continue creating the file.
    __On *nix, including OS X__
    Save the file as:
    ```bash
    ~/.splunkrc
    ```

> Notes:
> - Storing login credentials in the .splunkrc file is only for convenience during development—this file isn't part of the Splunk platform and shouldn't be used for storing user credentials for production. And, if you're at all concerned about the security of your credentials, just enter them at the command line and don't bother using the .splunkrc file.
> - The format of the .splunkrc file has changed between releases. If you are using a preview or beta version of the SDK, some of the newer fields might not be recognized and you might see errors while running the examples. You can either update to the latest version of the SDK, or comment out the app, owner, and version fields.
> - The version field is only used by the Splunk SDK for JavaScript.

For troubleshooting information about the .splunkrc file, see "[Troubleshooting](http://dev.splunk.com/view/python-sdk/SP-CAAAEUQ)".