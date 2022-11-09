Wiki: 
# How To Write our APIs? - Draft

## Introduction
[Summary Slides]

## General Remarks and Style Guide
APIs are defined according to the [OpenAPI Specification (OAS)](https://swagger.io/specification/). OAS specifies OpenAPI documents in either YAML or JSON style. SFTI APIs follows the YAML style. Besides the general Schema (c.f. [Swagger documentation](https://swagger.io/specification/#schema)) we make some additional conventions for reasons of consistency.  
Use just one item per line for any kind of lists and dictionaries to achieve a better readability. For example, 
```
enum: [a, b, c]
``` 
becomes 
```
enum:
    - a
    - b
    - c
``` 
Futhermore, use strings without quotes whenever possible, i.e. use `key: value string` instead of `key: 'value string'`. If quotes can not omited we use single quotes to fit strings together, for example `key: 'This value : requires quotes'`.

If parameters or other blocks of the API is used multiple times, you may consider use references to reuse code. As a rule of thumb, we recommend using references from a 3 times occurrence onwards to improve code maintainability and uniformity. 



## How to Document? Our Documentation Guide

In general, we use the approach to document with inline descriptive strings. 
For this purpose the _description_ field can be used.
As far as useful, every field should contain a description with a short, non-redundant statement of purpose.  

Use summary tags to summarize an endpoint's task. Summary should be written bullet-point like and with no point at the end of the sentence.
The overall description should provide more details of the task than the summary. The description contains a few sentences ending with a point.
Other description tags like parameter descriptions may end with or without point.  

In addition, parameter fields should also provide an _example_ field, and a _default_ OR _required_ field if applicable.
Be aware that the _required_ keyword overwrites default fields since the parameter must be set by the user.
All those fields should be used extensively.



Optional Feature:  
Automated documentation creating tools like Swagger UI support basic markdown for description fields. Unfortunately, online swagger UI (see [here](https://www.common-api.ch/index.php/de/resources-de/swagger-files#/)) does _not_ support this feature. Nevertheless, it my be helpful to use markdown to highlight different or important details.
Markdown should be used as follows.
- for now, use markdown as it seems fit
- tbd

## Nice to Have

From now on, we highly suggest to implement a order of fields. That means all fields are order by a specific rule and will always occur in this way. Omitted fields will be ignored and the order will preserved without those.  
1. The _summary_ field is always before the _description_ field since the summary gives an overall description.
2. For parameters we will use the following order:
```
parameters:
  - in: path
    name: exampleParameter
    schema:
      type: data type
      minimum: min
      maximum: max
      pattern: '[A-Za-z]{0-9}'
      default: default value
    required: true
    example: this parameter looks like this 
    description: path parameter
```
  


## Example
In this section we provide a schematic example that contains a full and well defined API according to this style and documentation guide.
```
openapi: "3.0.0"
info:
  version: 1.0.0
  title: Example API
  description:
    This API deserves as an example where you find all rules from the guide applied
  contact:
    email: max@muster.ch
  license:
    name: Apache 2.0
    url: 'http://www.apache.org/licenses/LICENSE-2.0.html'
servers:
  - url: https://example.api.ch
externalDocs:
  description: Find out more about this example API above
tags:
  - name: Example Tag
    description: provide details about this tag
paths:
/example-case/{exampleParameter}:
  get:
    summary: Brief summary of this endpoint
    description: More detailed description what happens here, e.g. purpose, how to use, required parameter
    tags:
      - Example Tag
    parameters:
      - in: path
        name: exampleParameter
        schema:
          type: string
        required: true
        example: this parameter looks like this 
        description: path parameter
      - in: query
        name: language
        schema:
          type: string
          enum:
            - de
            - fr
            - it
            - en
          default: en
        example: fr
        description: Language of the example API
      - $ref: '#/components/parameters/example'
    responses:
        '200':
        description: OK
        content:
            application/json:
        headers:
            Example-Header:
            schema:
                type: string
            description: Example header set in response
components:
  parameters:
    example: 
      in: query
      name: globalExample
      schema:
        type: integer
        minimum: 0
        maximum: 20
        default: 3
      example: 10
      description: 'Global example parameter (SCOPE: Ex). Note, use quotes here due to the colon.'


```


