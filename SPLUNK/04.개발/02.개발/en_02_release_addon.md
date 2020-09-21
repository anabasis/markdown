# Release an app or add-on

- Package your app
- Get your app inspected
- Publish your app
- Get your app certified for Splunkbase
- Get your app vetted for Splunk Cloud

## Package your app

An easy way to deploy your Splunk app (or add-on) to other computers is to install the app from a package. A packaged app is is a compressed tar archive (a .tar.gz tarball) with the .spl file extension that includes the dashboards, reports, scripted and modular inputs, field extractions, workflow actions, lookups, event types, and other objects in your app. Users can install your packaged app directly using Splunk Web or the command line, and organizations can use deployment tools to distribute apps to their users.

The Packaging Toolkit provides tools for authoring, packaging, and validating a Splunk app to help improve app management, including installation, configuration, and updating the app.

> Learn more about the [Splunk Packaging Toolkit](http://dev.splunk.com/view/packaging-toolkit/SP-CAAAE9V)
[개발/08.PKG_TOOLKIT](../../개발/08.PKG_TOOLKIT/en_pkg_toolkit.md)

## Get your app inspected

Splunk AppInspect evaluates your Splunk app against a set of Splunk-defined criteria so that you can be assured of its quality and robustness. AppInspect runs various checks on your app package, and then produces a report that clearly details any missed criteria.

> Learn more about [Splunk AppInspect](http://dev.splunk.com/view/appinspect/SP-CAAAE9U)
[개발/09.APPINSPECT](../../개발/09.APPINSTECT/appinspect.md)

## Publish your app

You can share your app with the world by submitting it to Splunkbase, the Splunk app and add-on marketplace. You can download apps and add-ons for use in your Splunk environment, and you can create your own Splunk apps and share them with other members of the Splunk community.

When you are ready to submit your app to Splunkbase, refer to Submit content to Splunkbase in the Working with Splunkbase manual for directions.

> Read about Splunkbase in the [Working with Splunkbase](http://docs.splunk.com/Documentation/Splunkbase/splunkbase/Splunkbase/Introduction) manual

## Get your app certified for Splunkbase

You can obtain Splunk Certification for apps and add-ons, which means that Splunk has examined an app or add-on and found that it conforms to best practices for Splunk development. By certifying your app or add-on after publishing it to Splunkbase, you give your users the confidence of knowing that Splunk has analyzed your app or add-on according to a strict set of criteria.

> Learn more about [Splunk App Certification](http://dev.splunk.com/view/app-cert/SP-CAAAE2S)

## Get your app vetted for Splunk Cloud

Apps running in Splunk Cloud undergo a vetting process before they can be installed. This process ensures that the app does not compromise security or pose an operations risk in the public cloud environment.

> Learn more about [Vetting apps and add-ons for Splunk Cloud](http://dev.splunk.com/view/app-cert/SP-CAAAE85)