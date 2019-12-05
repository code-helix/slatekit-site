---
title: "Queues"
date: 2019-11-17T23:55:42-05:00
section_header: Queues
---

# Overview
The Files component is an abstraction of persistent queues with a default implementation using **AWS SQS**. This also provides a much simplified API while making the underlying implementation swappable.


# Status
This component is currently **stable**. Following limitations, current work, planned features apply.
<table class="table table-bordered table-striped">
    <tr>
        <td><strong>Feature</strong></td>
        <td><strong>Status</strong></td>
        <td><strong>Description</strong></td>
    </tr>
    <tr>
        <td>**Meta**</td>
        <td>Upcoming</td>
        <td>Ability to easily default / inject meta data for every entry.</td>
    </tr>
    <tr>
        <td>**URI**</td>
        <td>Upcoming</td>
        <td>Ability to use Slate Kit Uri for explicit references to file paths</td>
    </tr>
    <tr>
        <td>**Errors**</td>
        <td>Upcoming</td>
        <td>Ability to use a dead-letter queue for failed items.</td>
    </tr>
    <tr>
        <td>**Batch**</td>
        <td>Upcoming</td>
        <td>Ability to perform batch insert operations</td>
    </tr>
</table>
{{% section-end mod="arch/queues" %}}

# Install
{{< highlight groovy >}}

    repositories {
        // other repositories
        maven { url  "http://dl.bintray.com/codehelixinc/slatekit" }
    }

    dependencies {
        // other dependencies ...

        compile 'com.slatekit:slatekit-cloud:1.0.0'
    }

{{< /highlight >}}
{{% sk-module 
    name="Files"
    package="slatekit.cloud"
    jar="slatekit.cloud.jar"
    git="https://github.com/code-helix/slatekit/tree/master/src/lib/kotlin/slatekit-cloud"
    gitAlias="slatekit/src/lib/kotlin/slatekit-cloud"
    url="arch/queues"
    uses="slatekit.results, slatekit.core, slatekit.cloud"
    exampleUrl=""
    exampleFileName="Example_Files.kt"
%}}
{{% section-end mod="arch/queues" %}}

# Requires
This component uses the following other <strong>Slate Kit</strong> and/or third-party components.
<table class="table table-bordered table-striped">
    <tr>
        <td><strong>Component</strong></td>
        <td><strong>Description</strong></td>
    </tr>
    <tr>
        <td>{{% sk-link-arch page="results" name="Results" %}}</td>
        <td>To model successes and failures with optional status codes</td>
    </tr>
    <tr>
        <td>{{% sk-link-util page="overview" name="Utils" %}}</td>
        <td>Common utilities for both android + server</td>
    </tr>
</table>
{{% section-end mod="arch/queues" %}}

# Imports
{{< highlight kotlin >}}
         
    import slatekit.cloud.aws.AwsCloudQueue
    import slatekit.common.queues.QueueStringConverter
     
{{< /highlight >}}

{{% section-end mod="arch/queues" %}}

# Setup
{{< highlight kotlin >}}
        
    val converter = QueueStringConverter()
    // Not storing any key/secret in source code for security purposes
    // Setup 1: Use the default aws config file in "{user_dir}/.aws/credentials"
    val queue1 = AwsCloudQueue<String>("app1-queue-1", "queue1", converter)

    // Setup 2: Use the type safe config in "{user_id}/myapp/conf/queue.conf"
    // Reads from the section "sqs" by default
    val queue2 = AwsCloudQueue<String>("app1-queue-1", "queue1", converter,"user://myapp/conf/queue.conf")

    // Setup 3: Use the type safe config in "{user_id}/myapp/conf/queue.conf"
    // Reads from the section supplied "sqs-3" ( if you have multiple sqs configurations )
    val queue3 = AwsCloudQueue<String>("app1-queue-1",  "queue1", converter, "user://myapp/conf/queue.conf", "sqs-1")
     
{{< /highlight >}}

{{% section-end mod="arch/queues" %}}

# Usage
{{< highlight kotlin >}}
        
    // Use case 1: init()
    queue2.init()

    // Use case 2: send 1 message
    queue2.send("item 1")

    // Use case 3: send multiple messages
    queue2.send("item 2")

    // Use case 4: send message with tags
    queue2.send("user=kishore", tagName="type", tagValue="reg")

    // Use case 5: receive 1 message
    val item1 = queue2.next()
    println(item1?.getValue())
    println(item1?.getTag("type"))

    // Use case 6: recieve 2 messages
    val items = queue2.next(2)

    // Use case 7: delete a message
    queue2.complete(item1)

    // Use case 8: delete many
    queue2.completeAll(items)

    // Use case 9: abandon a message
    queue2.abandon(queue2.next())

    // Use case 10: get count ( approximation )
    val count = queue2.count()
    println(count)
      

{{< /highlight >}}
{{% section-end mod="arch/queues" %}}

<script>
    var archComponent = {
        name: "Queues",
        page: "arch/queues",
        icon: "assets/media/img/white/queue.png",
        menu: {
            mode: "normal",
            useTemplate:true,
            sections: [
                {
                    name: "Guide",
                    items: [
                        { name:"Setup" , anchor: "#setup" },
                        { name:"Usage" , anchor: "#usage" }
                    ]
                }
            ]
        }
    };

    function setupArchComponent() {
        buildArchComponent(archComponent);
    }
</script>

