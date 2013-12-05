# Aon Build Tools (Gradle) #

This project consists of several Plugins for Gradle, all were developed by Aon eSolutions in order to provide more efficient and readable build files.  Since there is no proprietary code in here, it has been released under the Apache-2.0 License for all to use.

## Exec Plugin ##

Enable this plugin by: `apply plugin: 'exec'`

See examples in: `pluginTestExec.gradle`

This plugin enables easy execution of programs on the host OS.  It works by finding the executable on the path, or by a specified location.  There are several built in executables:

- Java - The java executable (JavaExecutable)
- Npm - The NPM (Node Packaged Modules) executable.  Useful for installing Node.JS Scripts (NpmExecutable)
- NodeJS - Node JS executable.  Very useful for compiling JavaScript tasks (NodeJSExecutable)
- PhantomJS - A Headless browser. (PhantomJSExecutable)

Each of these executables extend the Executable class.  Please see the GroovyDoc for each of the classes for any methods you can call.  For example, on the Java executable, you can do either of the following:

Execute Java Directly (with arguments and callbacks for Standard Out / Standard Error):

	java.run(["-version"], {
        println "Std Out: $it"
    }, {
        println "Std Err: $it"
    })

Or use a built in method:

	println "Version: ${java.getVersion()}"

There are also two containers that you can extend as well:

### Executables ###

This is a container so you can define any executables that you want.  The idea here is that you define the executable in a definition and then use that executable in a task.  For example:

	executables {
		gradle {
			location = "gradlew.bat"
		}
	}

Then in a task, you should be able to do the following:

	executables.gradle.run("--version")

PLEASE NOTE: This functionality is not working yet, but is planned.

### Node JS Scripts ###

Node JS Scripts are the primary reason why this plugin was developed.  It provides for a VERY simple way of executing NodeJS scripts and doing various tasks like Karma testing.  An example of the configuration would be like the following:

	nodeJsScripts {
	    karma {
	        scriptLocation = file("node_modules/karma/bin/karma")
	        standardOutLogLevel = "DEBUG"
	        standardErrorLogLevel = "ALWAYS"
	    }
	
	    plato {
	        standardOutLogLevel = "DEBUG"
	        standardErrorLogLevel = "ALWAYS"
	    }
	}

Then, in a task, you can do something like the following:

	nodeJsScripts.karma.run("start", pathToKarmaConfig, "--no-auto-watch", "--browsers", "PhantomJS", "--single-run")

In the NodeJSScript, you can configure several things (all of which are OPTIONAL):

- scriptLocation - Location of the script.  Defaults to asking NPM where the script exist.
- global - Should this be installed from NPM globally (true) or locally (false).  Defaults to false.
- scriptArguments - Arguments that should be provided when calling run.  These will OVERRIDE any arguments passed to the run method
- environmentVariables - Map of variables to place in the running environment.
- standardOutLogLevel - INFO, DEBUG, ALWAYS, or NONE.  Level to log the Standard output.  Defaults to NONE
- standardErrorLogLevel - INFO, DEBUG, ALWAYS, or NONE.  Level to log the Standard error.  Defaults to NONE 