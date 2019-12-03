---
title: "Start"
date: 2019-03-17T13:02:30-04:00
draft: true
---

{{% heading name="Overview" %}}
To get started with Slate Kit, first take a look at the overview page. 
This will provide info on the goals, use-cases, technology, philosophy
and the components offered.
{{% break %}}

{{% heading name="Index" %}}
Table of contents for this page
<table class="table table-bordered table-striped">
    <tr>
        <td><strong>Section</strong></td>
        <td><strong>Component</strong></td>
        <td><strong>Description</strong></td>
    </tr>
    <tr>
        <td><strong>1</strong></td>
        <td><strong><a class="url-ch" href="core/cli#status">Install</a></strong></td>
        <td>Current status of this component</td>
    </tr>
    <tr>
        <td><strong>2</strong></td>
        <td><strong><a class="url-ch" href="core/cli#status">Setup</a></strong></td>
        <td>Current status of this component</td>
    </tr>
    <tr>
        <td><strong>3</strong></td>
        <td><strong><a class="url-ch" href="core/cli#install">Available</a></strong></td>
        <td>Overview of all the modules available</td>
    </tr>
    <tr>
        <td><strong>4</strong></td>
        <td><strong><a class="url-ch" href="core/cli#requires">Results</a></strong></td>
        <td>Start with the Results component used throughout the project</td>
    </tr>
    <tr>
        <td><strong>5</strong></td>
        <td><strong><a class="url-ch" href="core/cli#requires">Utilities</a></strong></td>
        <td>Use the common component for 24+ utilities for both Android + Server</td>
    </tr>
    <tr>
        <td><strong>6</strong></td>
        <td><strong><a class="url-ch" href="core/cli#requires">Concepts</a></strong></td>
        <td>Some general concepts before diving into the other modules</td>
    </tr>
    <tr>
        <td><strong>7</strong></td>
        <td><strong><a class="url-ch" href="core/cli#requires">Components</a></strong></td>
        <td>All the components available and their artifacts</td>
    </tr>
    <tr>
        <td><strong>8</strong></td>
        <td><strong><a class="url-ch" href="core/cli#requires">Samples</a></strong></td>
        <td>Lists all the Slate Kit and third-party dependencies</td>
    </tr>
</table>
{{% section-end mod="core/cli" %}}


{{% heading name="Install" %}}
All software below is required to run Kotlin and Slate Kit. Kotlin is dependent on Java. Gradle is used to build and package Kotlin projects and is the build tool for Slate Kit.
{{% break %}}
### Required
<table class="table table-bordered table-striped">
    <tr class="">
        <td><strong>Tech</strong></td>
        <td><strong>Version</strong></td>
        <td><strong>About</strong></td>
        <td><strong>Link</strong></td>
    </tr>
    <tr class="">
        <td><strong>Java</strong></td>
        <td>1.8</td>
        <td>Java</td>
        <td><a class="url-ch" href="#installation">see below</a></td>
    </tr>
    <tr class="">
        <td><strong>Kotlin</strong></td>
        <td>1.1.2</td>
        <td>Kotlin</td>
        <td><a class="url-ch" href="#installation">see below</a></td>
    </tr>
</table>
{{% break %}}
### Suggested
The software below is recommended but not required. Slate Kit supports MySql with support for PostGres and Sql Server coming later. Having the MySql Java Connector will help with the sample apps and is also needed for the ORM ( entities ).
<table class="table table-bordered table-striped">
    <tr class="">
        <td><strong>Tech</strong></td>
        <td><strong>Version</strong></td>
        <td><strong>About</strong></td>
        <td><strong>Link</strong></td>
    </tr>
    <tr>
        <td><strong>IntelliJ</strong></td>
        <td>2017.2</td>
        <td>IDE</td>
        <td><a class="url-ch" href="https://www.jetbrains.com/idea/download/">download</a></td>
    </tr>
    <tr>
        <td><strong>Gradle</strong></td>
        <td>3.5</td>
        <td>Build tool</td>
        <td><a class="url-ch" href="#installation">see below</a></td>
    </tr>
</table>
{{% break %}}
### Optional
Slate Kit supports building Web APIs using Spark Java, Cloud Services ( Files, Queues ) using AWS and databases using MySql. The following are needed if you plan on using any of these.
<table class="table table-bordered table-striped">
    <tr class="">
        <td><strong>Tech</strong></td>
        <td><strong>Version</strong></td>
        <td><strong>About</strong></td>
        <td><strong>Link</strong></td>
    </tr>
    <tr>
        <td><strong>MySql</strong></td>
        <td>5.7</td>
        <td>Database Server</td>
        <td><a class="url-ch" href="https://dev.mysql.com/downloads/mysql/">download</a></td>
    </tr>
    <tr>
        <td><strong>MySql Connector</strong></td>
        <td>5.7</td>
        <td>For database connectivity and orm</td>
        <td><a class="url-ch" href="https://dev.mysql.com/downloads/connector/j/">download</a></td>
    </tr>
    <tr>
        <td><strong>Spark Java</strong></td>
        <td>2.6</td>
        <td>For Slate API Server</td>
        <td><a class="url-ch" href="http://sparkjava.com/">visit</a></td>
    </tr>
    <tr>
        <td><strong>AWS Sdk</strong></td>
        <td>1.10.55</td>
        <td>File Storage( S3 ), Queues( SQS )</td>
        <td><a class="url-ch" href="https://aws.amazon.com/sdk-for-java/">download</a></td>
    </tr>
</table>
{{% section-end mod="core/cli" %}}


{{% heading name="Setup" %}}
You can reference all the Slate Kit modules in gradle. The binaries are available from <strong>bintray</strong>. 
First you just have to include the Slate Kit maven url.
Only the results, utilities modules are listed here for sample purposes. 
You can include additional projects
{{% break %}}
{{< highlight kotlin >}}

    // other setup ...
    repositories {
        maven { url  "https://dl.bintray.com/codehelixinc/slatekit" }
    }

    dependencies {
        // other libraries
        
        // slatekit-results: Result<T,E> to model successes/failures with optional status codes
        compile 'com.slatekit:slatekit-results:0.9.9'
        
        // slatekit-common: Utilities for Android or Server
        compile 'com.slatekit:slatekit-common:0.9.9'
        
        // Misc architecture components ( all depend on results/common components above )
        // Use only what you need ( see artifacts next section )

    }

{{< /highlight >}}
{{% section-end mod="core/cli" %}}


{{% heading name="Available" %}}
All the components available are located in various projects for the purpose of organization and modularity. Take a look at the projects to get an understanding of the purpose of each project and the dependencies between them. You can use as little of Slate Kit or as much as you need.
<strong>The simplest way to start is with the Results component, followed by the Utilities</strong>. Then you can proceed to checking out the concepts, sample apps and using just the individual libraries that you need.
{{% break %}}
{{% sk-modules %}}
{{% button url="" text="see overview" %}}
{{% break %}} 
{{% break %}}



{{% heading name="Projects" %}}
All the components available are located in various projects for the purpose of organization and modularity. Take a look at the projects to get an understanding of the purpose of each project and the dependencies between them. You can use as little of Slate Kit or as much as you need.
<strong>The simplest way to start is with the Results component, followed by the Utilities</strong>. Then you can proceed to checking out the concepts, sample apps and using just the individual libraries that you need.
{{% break %}}
{{% sk-modules-artificats %}}
{{% button url="" text="see overview" %}}
{{% break %}} 
{{% break %}}


{{% heading name="Samples" %}}
There are multiple examples, unit-tests, sample applications, and docs available to fully understand how to use the Slate Kit components.
{{% break %}}
{{% sk-resources %}}
{{% button url="" text="see overview" %}}
{{% break %}} 
{{% break %}}



