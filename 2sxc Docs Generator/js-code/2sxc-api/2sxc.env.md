---
uid: JsCode.2sxcApi.$2sxc.Env
---

# The $2sxc.env API

[!include[](~/basics/stack/_shared-float-summary.md)]
<style>.context-box-summary .interact-2sxc { visibility: visible; } </style>

The `$2sxc.env` object manages environment information for the JavaScript. _It's new - introduced in 2sxc 10.25_.

> [!NOTE]
> The `env`-object was introduced to help $2sxc run without jQuery and without the rather instable ServicesFramework of DNN. 

It has the following important methods:

* `page()` which will tell you the page number - often needed in API calls
* `api()` the root path for api-calls, used in [http](xref:JsCode.2sxcApi.$2sxc.Http)
* `rvt()` the request-verification token needed for internal WebAPI calls

Internally this information is automatically retrieved from the html-header. 
The environment looks for a special meta-tag called `_jsApi` which contains all this information. 
This is new in 2sxc 10.25 and was added to avoid using jQuery when not necessary. 

## Internal Stuff

The `env` also has some internal methods like `load(...)` for special scenarios like using $2sxc in an html which does not come from DNN. 
This is not documented in detail, but can be figured out by reading the code. 

For debugging, there is also a `log` object which contains some information how the `env` was built, how long it took etc. 

## History

1. Introduced in 2sxc 10.25
