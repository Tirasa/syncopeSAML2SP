# About this repository

This repository contains a sample Java EE web application leveraging the [SAML 2.0 Service Provider](https://ci.apache.org/projects/syncope/reference-guide.html#saml-2-0-service-provider) extension available since the upcoming Apache Syncope 2.0.3.

Once an Apache Syncope deployment - enabled with such extension - is properly configured, and the Syncope [Core](http://syncope.apache.org/docs/reference-guide.html#core) application is running, the Syncope [Admin UI](http://syncope.apache.org/docs/reference-guide.html#admin-console-component) and the Syncope [Enduser UI](http://syncope.apache.org/docs/reference-guide.html#enduser-component) can be enabled to allow SAML-based SSO.
The global result is that Admin UI and / or Enduser UI can be accessed after user authentication against (one of configured) SAML 2.0 Identity Provider(s).

**The web application in this repository shows how to enable any 3rd party Java EE web application to act like as the Syncope Admin UI and Enduser UI.**
