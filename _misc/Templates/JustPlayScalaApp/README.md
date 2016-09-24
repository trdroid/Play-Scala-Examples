### Creating the project

**On Ubuntu**

```sh
~/onGit/Play-Scala-Examples/_misc/Templates$ which activator
/home/droid/software/typesafe-activator/activator-dist-1.3.10/bin/activator
~/onGit/Play-Scala-Examples/_misc/Templates$ activator new JustPlayScalaApp just-play-scala

Fetching the latest list of templates...

OK, application "JustPlayScalaApp" is being created using the "just-play-scala" template.

To run "JustPlayScalaApp" from the command line, "cd JustPlayScalaApp" then:
/home/droid/onGit/Play-Scala-Examples/_misc/Templates/JustPlayScalaApp/activator run

To run the test for "JustPlayScalaApp" from the command line, "cd JustPlayScalaApp" then:
/home/droid/onGit/Play-Scala-Examples/_misc/Templates/JustPlayScalaApp/activator test

To run the Activator UI for "JustPlayScalaApp" from the command line, "cd JustPlayScalaApp" then:
/home/droid/onGit/Play-Scala-Examples/_misc/Templates/JustPlayScalaApp/activator ui
```

### Project structure and content

![](_misc/Project%20Structure%20and%20Content.png)

### Understanding content

**Controller files**

*JustPlayScalaApp/app/controllers/Application.scala*

```scala
package controllers

import play.api.mvc._

object Application extends Controller {

  def index = Action {
    Ok(views.html.main())
  }

}
```

The *Action* and *Ok* methods are imported from the *play.api.mvc* package. 

**View files**

The code for views reside in the *app/views* directory.

The generated view file is a *Twirl* template, which facilitates the views to be defined using Scala code along with HTML (which is why the generated view file has the extension *.scala.html* rather than just *html*)

*JustPlayScalaApp/app/views/main.scala.html*

```scala
<!DOCTYPE html>

<html>
    <head>
        <title>Just Play Scala</title>
    </head>
    <body>
        <h1>Just Play Scala</h1>
    </body>
</html>
```

**Configuration file**

Application level configuration properties are set in the following file.

*JustPlayScalaApp/conf/application.conf*

```conf
# This is the main configuration file for the application.
# ~~~~~

# Secret key
# ~~~~~
# The secret key is used to secure cryptographics functions.
# If you deploy your application to several instances be sure to use the same key!
application.secret="bx]?^7f:lTdpF:qQ/Jh[7Y]d]N>W;ESPI7TD]uYDRte[ApO6k3[w]lvWX_DjTh`h"

# The application languages
# ~~~~~
# application.langs="en"

# Global object class
# ~~~~~
# Define the Global object class for this application.
# Default to Global in the root package.
# application.global=Global

# Router
# ~~~~~
# Define the Router object to use for this application.
# This router will be looked up first when the application is starting up,
# so make sure this is the entry point.
# Furthermore, it's assumed your route file is named properly.
# So for an application router like `my.application.Router`,
# you may need to define a router file `conf/my.application.routes`.
# Default to Routes in the root package (and conf/routes)
# application.router=my.application.Routes

# Database configuration
# ~~~~~
# You can declare as many datasources as you want.
# By convention, the default datasource is named `default`
#
# db.default.driver=org.h2.Driver
# db.default.url="jdbc:h2:mem:play"
# db.default.user=sa
# db.default.password=""

# Evolutions
# ~~~~~
# You can disable evolutions if needed
# evolutionplugin=disabled

# Logger
# ~~~~~
# You can also configure logback (http://logback.qos.ch/),
# by providing an application-logger.xml file in the conf directory.

# Root logger:
logger.root=ERROR

# Logger used by the framework:
logger.play=INFO

# Logger provided to your application:
logger.application=DEBUG
```

**Routes file**

All requests that the application supports should be defined in the following file. 

*JustPlayScalaApp/conf/routes*

```conf
# Routes
# This file defines all application routes (Higher priority routes first)
# ~~~~

# Home page
GET     /                           controllers.Application.index

# Map static resources from the /public folder to the /assets URL path
GET     /assets/*file               controllers.Assets.versioned(path="/public", file)
```

It can be noticed that each route definition has the following parts

* Request method (GET, PUT, POST, DELETE)
* Path to which to respond to
* Method that should be invoked to return a response

The first entry from the above file could be interpreted as:

A GET request to the path "/" would invoke the method "controllers.Application.index" and responds to the request with the return value of the method.

The second entry from the above file could be interpreted as:

A GET request to a the path "/assets/*file" would be served from the "public" folder.

The *public* directory is where Play-independent resources like JavaScript, CSS, images etc. are hosted

**Build files**

To indicate SBT that the project is a Play application and that it "recognizes" the project as a Play application, it has to be provided with the Play settings. This is done by enabling the *PlayScala* plugin from where the Play settings are imported.

*JustPlayScalaApp/build.sbt*

"build.sbt" is the build definition file, which is essentially a list of key-value pairs. The symbol ":=" is the assignment operator. The default build file includes the following keys

* name
* version
* root

```sbt
name := """JustPlayScalaApp"""

version := "1.0-SNAPSHOT"

lazy val root = project.in(file(".")).enablePlugins(PlayScala)
```

SBT allows build definitions to be extended using plugins. To allow SBT to download the plugins required for Play applications, the following content is generated by the template

*JustPlayScalaApp/project/plugins.sbt*

```properties
resolvers += "Typesafe repository" at "http://repo.typesafe.com/typesafe/releases/"

addSbtPlugin("com.typesafe.play" % "sbt-plugin" % "2.3.4")
```

The *resolver* has to be modified if the plugin is not hosted on Maven or Typesafe repositories.

The *addSbtPlugin* is passed the Ivy module ID for the plugin.

**Addressing SBT version incompatibilities**

The generated build properties contains the version of the SBT being used. This avoids any incompatibility issues when the same build definition is compiled by different SBT versions. 

*JustPlayScalaApp/project/build.properties*

```properties
#Activator-generated Properties
#Sat Sep 24 13:15:17 EDT 2016
template.uuid=71358ad9-fc02-4dfa-aa4a-1ae1964e5181
sbt.version=0.13.5
```
