# About this repository

This repository contains a sample Java EE web application leveraging the [SAML 2.0 Service Provider](https://ci.apache.org/projects/syncope/reference-guide.html#saml-2-0-service-provider) extension available since the upcoming Apache Syncope 2.0.3.

Once an Apache Syncope deployment - enabled with such extension - is properly configured, and the Syncope [Core](http://syncope.apache.org/docs/reference-guide.html#core) application is running, the Syncope [Admin UI](http://syncope.apache.org/docs/reference-guide.html#admin-console-component) and the Syncope [Enduser UI](http://syncope.apache.org/docs/reference-guide.html#enduser-component) can be enabled to allow SAML-based SSO.
The global result is that Admin UI and / or Enduser UI can be accessed after user authentication against (one of configured) SAML 2.0 Identity Provider(s).

**The web application in this repository shows how to enable any third party Java EE web application to act like as the Syncope Admin UI and Enduser UI, e.g. to allow SAML-based SSO.**

# Preparation

## Apache Syncope

First of all, an Apache Syncope deployment must be set up:

1. download the SNAPSHOT [Standalone Distribution](https://repository.apache.org/content/groups/snapshots/org/apache/syncope/syncope-standalone/2.0.3-SNAPSHOT/syncope-standalone-2.0.3-20170413.120848-103-distribution.zip)
1. put it [at work](http://syncope.apache.org/docs/getting-started.html#standalone)
1. access http://localhost:9080/syncope-console and log in as `admin` / `password`

_The procedure above is just the most straightforward to get quickly Apache Syncope up and running, provided with the required extension: for any production environment, it is **strongly** suggested to proceed with [Maven project generation from archetype](http://syncope.apache.org/docs/getting-started.html#maven-project), instead._

### Import SAML 2.0 Identity Provider metadata into Syncope

From Admin UI, go to `Extensions > SAML 2.0 SP > Identity Providers` and click on the `+` button on the lower right corner: you can then upload the metadata provided by your SAML 2.0 Identity Provider of choice.
There are free ones available, as [TestShib](https://www.testshib.org/) or [SSOCircle](https://www.ssocircle.com), or you might want to try yourself with a [Docker-ized SimpleSAMLPHP instance](https://hub.docker.com/r/kristophjunge/test-saml-idp/).

Now click on the pencil icon to finetune the IdP configuration. You can optionally change its label from the first screen, but the most important setting is revealed after hitting the `Next` button: you will need to define the mapping between internal users (e.g. users in Apache Syncope) and external users (e.g. users from the IdP); some sample values are reported below:

SAML 2.0 Identity Provider | Internal attribute | External attribute
--- | --- | ---
[TestShib](https://www.testshib.org/) | `username` | `uid`
[SSOCircle](https://www.ssocircle.com) | `email` | `EmailAddress`
[Docker-ized SimpleSAMLPHP instance](https://hub.docker.com/r/kristophjunge/test-saml-idp/) | `email` | `email`

Finally, be sure that users exist in Syncope matching the ones you are targeting from Identity Providers.

## Java EE web application

Adjust the content of `src/main/resources/saml2sp-agent.properties` to match your actual Apache Syncope deployment, then

```
$ mvn tomcat7:run
```
At this point the web application is available at http://your.host.name:8080/syncopeSAML2SP/

### Send metadata to the SAML 2.0 Identity Provider

Access http://your.host.name:8080/syncopeSAML2SP/saml2sp/metadata and an XML response will be shown.

Now hand off such XML content to your reference SAML 2.0 Identity Provider, as said above.

_Please note that the actual URLs contained in the metadata will automatically adjust depending on the hostname referenced for access: within this regard, using `localhost` is discouraged in favor of FQDN as http://your.host.name:8080/syncopeSAML2SP/saml2sp/metadata_

# Run

1. Browse to http://your.host.name:8080/syncopeSAML2SP/saml2sp/login
1. You will be redirected to the selected IdP's login form
1. Log in with the provided credentials
1. You will be greeted by a similar page (this was obtained from TestShib after logging in as `myself`): ![GitHub Logo](/images/TestShib.png)
