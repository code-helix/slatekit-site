---
layout: start_page_mods_utils
title: module Templates
permalink: /kotlin-mod-templates
---

# Templates

{: .table .table-striped .table-bordered}
|:--|:--|
| **desc** | A micro template system for processing text with variables, useful for generating dynamic emails/messages. | 
| **date**| 2018-03-19 |
| **version** | 0.9.9  |
| **jar** | slatekit.common.jar  |
| **namespace** | slatekit.common.templates  |
| **source core** | slatekit.common.templates.Templates.kt  |
| **source folder** | [src/lib/kotlin/slatekit/](https://github.com/code-helix/slatekit/tree/master/src/lib/kotlin/slatekit/){:.url-ch}  |
| **example** | [/src/apps/kotlin/slate-examples/src/main/kotlin/slatekit/examples/Example_Templates.kt](https://github.com/code-helix/slatekit/tree/master/src/lib/kotlin/slatekit-examples/src/main/kotlin/slatekit/examples/Example_Templates.kt){:.url-ch} |
| **depends on** |   |

## Import
```kotlin 
// required 
import slatekit.common.templates.Template
import slatekit.common.templates.TemplateConstants
import slatekit.common.templates.TemplatePart
import slatekit.common.templates.Templates



// optional 
import slatekit.core.cmds.Cmd
import slatekit.common.Result
import slatekit.common.Random



```

## Setup
```kotlin


    // Setup the templates with list of predefined(named) templates and
    // also a list of handlers to process named variables in the templates.
    // NOTE: The Templates.apply method ( used here ) processes/parses the templates
    // first so they don't have to be processed on demand.
    // You can also use the Templates constructor directly.
    val templates = Templates.build(
      templates = listOf(
        Template("welcome", "Hi @{user.api}, Welcome to @{company.api}."),
        Template("confirm", "Your confirmation code for @{app.api} is @{code}.")
      ),
      subs = listOf(
        Pair("user.home"    , { s -> "c:/users/johndoe"                } ),
        Pair("company.api" , { s -> "CodeHelix"                       } ),
        Pair("company.dir"  , { s -> "@{user.home}/@{company.id}"      } ),
        Pair("company.confs", { s -> "@{user.home}/@{company.id}/confs"} ),
        Pair("app.api"     , { s -> "slatekit.sampleapp"              } ),
        Pair("app.dir"      , { s -> "@{company.dir}/@{app.id}"        } ),
        Pair("app.confs"    , { s -> "@{app.dir}/confs"                } ),
        Pair("user.api"    , { s -> "john.doe"                        } ),
        Pair("code"         , { s -> Random.alpha6()                   } )
    ))
    

```

## Usage
```kotlin


    // Case 1: Parse text and get the result as individual parts
    val result = templates.parse("Hi @{user.api}, Welcome to @{company.api}.")
    println("Case 1: Parse into individual parts")
    print(result.value)

    // Case 2: Parse text into a template
    val template = templates.parseTemplate("welcome", "Hi @{user.api}, Welcome to @{company.api}.")
    println("Case 2: Parse into a Template containing the parts")
    print(template)

    // Case 3: Resolve ( process ) the template on demand using the initial variables
    val text3 = templates.resolve("Hi @{user.api}, Welcome to @{company.api}.")
    println("Case 3: Resolve template with variables into a Option[String]")
    println(text3)
    println()

    // Case 4: Resolve ( process ) the template on demand using custom variables
    val text4 = templates.resolve("Hi @{user.api}, Welcome to @{company.api}.",
      Templates.subs (
        listOf(
          Pair("company.api" , { s -> "Gotham"  }),
          Pair("user.api"    , { s -> "batman"  })
        )
      )
    )
    println("Case 4: Resolve template with custom variables")
    println(text4)
    println()

    // Case 5: Resolve ( process ) saved template using the initial variables
    val text5 = templates.resolveTemplate("welcome")
    println("Case 5: Resolve named template")
    println(text5)
    println()

    // Case 6: Resolve ( process ) saved template using custom variables
    val text6 = templates.resolveTemplate("welcome", Templates.subs (
      listOf(
        Pair("company.api" , { s -> "Gotham"  } ),
        Pair("user.api"    , { s -> "batman"  } )
      )
    ))
    println("Case 6: Resolve named template with custom variables")
    println(text6)
    println()

    

```


## Output

```bat
  Case 1: Parse into individual parts
  type: txt, pos : 0, len : 3, text: Hi
  type: sub, pos : 5, len : 14, var: user.api
  type: txt, pos : 15, len : 13, text: , Welcome to
  type: sub, pos : 30, len : 42, var: company.api
  type: txt, pos : 43, len : 1, text: .

  Case 2: Parse into a Template containing the parts
  api : welcome, content : Hi @{user.api}, Welcome to @{company.api}., group : None, path : None, parsed : true, valid : true, status : None
  type: txt, pos : 0, len : 3, text: Hi
  type: sub, pos : 5, len : 14, var: user.api
  type: txt, pos : 15, len : 13, text: , Welcome to
  type: sub, pos : 30, len : 42, var: company.api
  type: txt, pos : 43, len : 1, text: .

  Case 3: Resolve template with variables into a Option[String]
  Some(Hi john.doe, Welcome to CodeHelix.)

  Case 4: Resolve template with custom variables
  Some(Hi batman, Welcome to Gotham.)

  Case 5: Resolve named template
  Some(Hi john.doe, Welcome to CodeHelix.)

  Case 6: Resolve named template with custom variables
  Some(Hi batman, Welcome to Gotham.)
```
