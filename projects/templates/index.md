---
layout: up
title: Project Templates
---

# Project Templates

Project templates define a set of properties for elements of a project. Once installed, any project assigned to a particular template will support those properties.

For example, in a Java project you can define a property for a Project element called "Class Path" allowing specification of the java class path for the project. Any external tool can then extract that value from the project file according to the pre-defined template.

Project templates are .pnpt files.

## Installation

To install a project template file, simply drop it (and any related files) in the "projects" directory under your PN install. Once you have done this, the template will be available inside PN for use when creating a new project. You can then create tools that will be available whenever a project of that type is open.

## Creation

There is a beta project template editor available here: http://pnotepad.org/files/templateeditor.zip

## Using

### Use in tool definitions

You can reference project and file properties when building parameters for a tool, simply use the `$(ProjectProp:x)` or `$(FileProp:x)` strings. Replace 'x' with the path to the property you want, in the form `"group\category\value"` replacing each part with values from the project template definition. For example with the example properties below you might use `$(ProjectProp:test\general\outputdir)`.

### Use in an external tool

Once a user has set settings according to a project template, those values are stored in the .pnproj project file alongside the items the refer to. For example, the following project node from a .pnproj file contains values related to the example template from this documentation:

```xml
<Project 
    name="blah" 
    typeId="{087F8071-E230-4d0f-9B49-4C29BBE2D7A8}">

    <g1:test xmlns:g1="urn:test">
        <g1:general>
            <g1:outputdir>C:\projects\personal\scintilla</g1:outputdir>
            <g1:intermediate></g1:intermediate>
        </g1:general>
        <g1:general2>
            <g1:cheese>true</g1:cheese>
            <g1:add>false</g1:add>
        </g1:general2>
    </g1:test>
    <!-- ... Files and Folders from here on... -->
```

The first thing to note is the typeId attribute, this matches the unique ID of the project template (see example). This ID is used to match all the fields and template values when PN loads a project. The next thing to note is that all of the project properties are in namespaced nodes. The namespace used is specified in the template file (the ns attribute of the projectConfig element). The elements used match the group, category and individual values used in the template.

An external script or tool can easily extract these values to use them to process a PN project file and do something useful with it!

## Example Template

```xml
<?xml version="1.0"?>
<projectConfig 
    name="test" 
    id="{087F8071-E230-4d0f-9B49-4C29BBE2D7A8}" 
    ns="urn:test" 
    icon="test.ico" 
    helpfile="test.chm">

	<set type="project">
		<group name="test" description="Project Options">
			<category name="general" description="General Options">
				<folderPath name="outputdir" description="Output Directory" helpid="1023" />
				<filePath name="intermediate" description="Intermediate Directory" helpid="1024" />
			</category>
			<category name="general2" description="Another Category">
				<option name="cheese" description="Consume Cheese?" default="true"/>
				<option name="add" description="Add an option?" default="false"/>
			</category>			
		</group>
		<group name="test2" description="Defaults">
			<category name="defaults" description="Project Defaults">
				<optionlist name="configtype" description="Configuration Type" helpid="1025">
					<value description="Makefile" value="1" />
					<value description="Application (.exe)" value="2" />
					<value description="Dynamic Library (.dll)" value="3" />
					<value description="Static Library (.lib)" value="4" />
				</optionlist>
				<option name="browseinfo" description="Build Browser Information" helpid="2"/>
				<text name="testname" description="Project Name" default="some text here" helpid="1"/>
			</category>
		</group>
	</set>
	<set type="file">
		<group name="filetest" description="File Props">
			<category name="general" description="General Options">
				<option name="compile" description="Should Compile?" default="true"/>
				<int name="priority" description="Compile Priority" default="5"/>
			</category>
		</group>
	</set>
</projectConfig>
```