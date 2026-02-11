---
title: "The Super Sharp Blade"
date: 2026-02-06T21:45:00+01:00
draft: false
---

I love programming and people who know me also know that XSLT is my favorite language programming language.  Just today, I had the opportunity to use it again and I was reminded why I like it so much:  XSLT is the perfect tool for the tasks that it is designed for - It is a super sharp blade.

Today's task was an integration task:  I needed to devise a system in which the organizer of a conference could use a desktop vector graphics application (LibreOffice Draw) to place tables, identified by a unique number, on a floor plan.  That floor plan would then be imported into a web based system in which participants could claim tables for their use.

Thus, a way was needed to mark graphical elements in the floor plan as being tables and assigning them a unique number in a convenient way.  LibreOffice draw does not provide a way to assign user-defined attributes to graphical elements, but it provides a way to give them a name which would just be stored, but not displayed.  The table number would just be put into the rectangle's text content.  When exporting the floor plan to SVG, that name could be used to identify tables.

The resulting SVG element for table 124 would look something like this (attributes removed for clarity):

```xml
<g class="com.sun.star.drawing.CustomShape">
    <title>table
    </title>
    <g id="id2545">
        <rect />
        <path />
        <path />
        <text><tspan><tspan><tspan>124</tspan></tspan></tspan></text>
    </g>
</g>
```

The web system, however, needs the table number to be part of the id of the inner group, like so:
```xml
<g class="com.sun.star.drawing.CustomShape">
    <title>table
    </title>
    <g id="table_124">
        <rect />
        <path />
        <path />
        <text><tspan><tspan><tspan>124</tspan></tspan></tspan></text>
    </g>
</g>
```

The transformation task thus is to find group (g) elements that are preceded by a title element with the text "table" and to change the id of that group to include the table number, which itself is the text of the innermost tspan element.

The whole program, including all boilerplate, to perform this task is:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="3.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
                xmlns="http://www.w3.org/2000/svg"
                xpath-default-namespace="http://www.w3.org/2000/svg">

    <xsl:output method="xml"/>
    <xsl:mode on-no-match="shallow-copy"/>

    <xsl:template match="g[preceding-sibling::title[matches(text(), '^\s*table\s*$')]]">
        <xsl:variable name="table-number" select="normalize-space(.//tspan[matches(text(), '\d+')]/text())"/>
        <g id="table_{$table-number}">
            <xsl:apply-templates select="*"/>
        </g>
    </xsl:template>
</xsl:stylesheet>

```

I think this is beautiful for several reasons:

I/O is completely abstracted away from the transformation logic.  There is an implicit input and output document, but it does not need to be specified.  This removes a whole bunch of code that a more general purpose language would require for a program to do this.

Using the `xsl:mode` element, we can specify that all elements which are not matched by any of the templates are just copied to the output document unchanged.

XPath allows concise matching of the desired element.  One can express neighborhood relationships of elements in a single statement, and additional conditions for matching can be added, including regular expression matches.

In the matching rule, the document can be further traversed with XPath.  Here, we assign the table number, which is in a tspan child element with a numeric text body, to a variable.  We use that variable in a template expression to construct the desired id.  Then, we'll copy the children of the matched group to the output.

XSLT is a great example of a special purpose language that achieves great expressive power by restricting itself to certain use cases.  For some tasks, it is the perfect tool.  I won't deny that it has many gotchas, but my love for it is eternal.
