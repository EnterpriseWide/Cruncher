﻿		
Cruncher
=========

A CSS, LESS, Sass, JavaScript, and CoffeeScript Preprocessor for ASP.NET.

Cruncher makes optimising your resources easy. It can bundle unlimited combinations of remote and local CSS, LESS, Sass, JavaScript, and CoffeeScript files;
combining them, minifying them and caching them in the browser. Cruncher can handle nested css @import statements, re-maps relative resource urls and has a self cleaning 
cache should any changes be made to any of the referenced files.

If you use Cruncher please get in touch on my twitter [@james_m_south](https://twitter.com/james_m_south).

Feedback is always welcome.

Installation.
===============================
Installation is simple. A Nuget package is available [here][1]. 

  [1]: https://nuget.org/packages/Cruncher/


Cruncher Web.config Explained.
===============================

``` xml
<cruncher>
	<!-- Whether to allow remote downloads -->
	<!-- The maximum allowed remote file size in bytes -->
	<!-- The remote file download timeout in milliseconds -->
	<security allowRemoteDownloads="true" maxBytes="524288" timeout="300000">
		<!--
		A list of white-listed urls from which we are allowed to download and process remote files.
		The token value allows us to add the file reference without the risk of hitting IEs 1024 
		character url limit.
		-->
		<whiteList>
		<add token="jquery" url="http://ajax.googleapis.com/ajax/libs/jquery/2.0.3/jquery.js" />
		</whiteList>
	</security>
	<processing>
		<!-- The comma separated virtual paths to the css and js folders.-->
		<virtualPaths cssPaths="~/css, ~/content" jsPaths="~/js, ~/scripts"/>
        <!-- Where to store crunched files in the application and how long to keep expired ones.-->
        <physicalFiles path="~/assets-cruncher" daysBeforeRemoveExpired="7"/>
	</processing>
</cruncher>
```

		
Cruncher Razor Helpers.
=======================

Cruncher comes with some helpers to make adding methods easier.

Simply add `@using Cruncher.Web` to your view and use the following methods to add resources.

``` csharp
@CruncherBundler.RenderCSS("style.css", "style.less", "style.scss")

@CruncherBundler.RenderJavaScript("jquery-2.0.3.js", "test.coffee", "test.js")
```

Cruncher Console Instructions.
==============================

Cruncher Console allows you to combine and minify .css, .less, .sass, .scss, .js, and .coffee files and entire folders containing those 
files from within the Visual Studio Package Manager console.

**Usage:**

    cruncherconsole.exe <operation switch> -t:<type> -in:<inputPath> -out:<outputPath>

**Parameters:**

	-m : Minifies files in the set.

	-t: specifies the output type of the file (css or javascript).
  
	-in : Input path. This can be a single file or a folder. If it is a folder, all files within will be processed 
	      and bundled into one output file.

	-out : Output path. The path to save crunched output to. If left blank then the input file is used as a 
	       reference for naming the output.

**Example:**

Building style.css (single file containing @import statements)
    
    cruncherconsole.exe -in:..\..\src\css\style.css -out:..\..\rel\style.css -t:css 

Building and minifying style.css
    
	cruncherconsole.exe -in:..\..\src\css\style.css -out:..\..\rel\style.min.css -t:css -m

Building a folder of JavaScript files.

    cruncherconsole.exe -in:..\..\src\js -out:..\..\rel\script.js -t:javascript 

Building and minifying a folder of JavaScript files.

    cruncherconsole.exe -in:..\..\src\js -out:..\..\rel\script.min.js -t:javascript -m
