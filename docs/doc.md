<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>

<style type="text/css">
body {
	font-family: verdana,arial,sans-serif;
}

#content {
	margin-left: 15px;
	max-width: 960px;;
}
h3, h4 {
	margin-bottom: 3px;
}
h4 {
	margin-top: .5em;
}
#footer {
	margin-top: 50px;
	padding: 5px;
	border-top: 1px solid #ccc;
	border-bottom: 1px solid #ccc;
}

</style>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>LITE documentation</title>
</head>
<body>
<h1>LITE (LoopIndex Tracking Enhancement) for CKEditor</h1>
<div id="content">
<h2>Installation</h2>
<div class="doc">Drop the two folders, <code>common</code> and <code>lite</code>, into the ckeditor <code>plugins</code> folder</div>

<h2>Getting Started</h2>
<div class="doc">
<ol>
<li>Include the lite plugin's interface in your html, e.g.
<pre>
	&lt;script type="text/javascript" src="ckeditor/plugins/lite/lite_interface.js"&gt;&lt;/script&gt;
</pre>
<li>Add the lite plugin to ckeditor. The simplest way to do this is by adding the following line to ckeditor's <code>config.js</code>:<pre>
	config.extraPlugins = 'lite';
</pre>
If your configuration already contains some plugin tweaking, just make sure that <code>lite</code> is included in the editor's plugins list.
<li>If you wish to make any other configuration changes via <code>config.js</code>, verify that the editor's configuation contains a <code>lite</code> section by adding the following lines to ckeditor's <code>config.js</code>:
<pre>
	var lite = config.lite|| {};
	config.lite = lite;
</pre>
<li>If your page doesn't include jQuery, the <code>lite</code> plugin will load it if you provide it with the relative path to the jquery source. If you are going to use the copy distributed with the plugin, add the following line to config.js:
<pre>
	config.lite.jQueryPath = "../common/jquery.min.js";
</pre>
</ol>
<h2>Configuration</h2>
<ul>
<li>By default, LITE loads an uncompressed version of <code>ice</code>, located in its <code>js</code> folder. To load <code>ice</code> from a different location, or load modified scripts, edit <code>config.js</code> and set a value to
<code>config.lite.includes</code>, e.g.
<pre>
	config.lite.includes = ["js/ice-all-sources.min.js", "js/plugins/ice-plugins.min.js"];
</pre>

<li>Select the <code>lite</code> commands you want to include in ckeditor's toolbar. By default all the commands in <code>lite_interace.js</code> are available in the toolbar. To configure this, edit <code>config.js</code> and set a value to the <code>config.lite.commands</code>, e.g.
<pre>
// display only the show/hide, accept all and reject all commands.
	config.lite.commands = [LITE.Commands.TOGGLE_SHOW, LITE.Commands.ACCEPT_ALL, LITE.Commands.REJECT_ALL];
</pre>
<li>Some more LITE options may be set through the <code>config.lite</code> object:
<ul>
<li><code>userName</code> - the initial username used by the <code>ICE</code> change tracker
<li><code>userId</code> - the initial user ID used by the <code>ICE</code> change tracker
<li><code>titleTemplate</code> - the template of the text shown as a title (tooltip) when the cursor hovers over a tracked change region. The default value is <code>Changed by %u %t</code>, <code>%u</code> representing the user name and <code>%t</code> representing a time stamp relative to the current time (e.g. <i>now</i>, <i>2 minutes ago</i>, <i>May 31</i>). The titles are refreshed periodically. If you provide an empty template, no title will be displayed.
</ul>
<li>You may also tweak the configuration in your code by listening to the CKEditor <code>configLoaded</code> event and then modifying the editor's <code>config.lite</code> object.
</ul>
</div>

<h2>Methods</h2>
<div class="doc">
<ul>
<li><h3>Get a reference to the lite plugin</h3>
To interact with the <code>lite</code> plugin, either access it through the editor's <code>plugins.lite</code> property, or listen to the event <code>LITE.Events.INIT</code> fired by the editor instance. The <code>data</code> member of the event will
contain a property called <code>lite</code> which references the lite plugin instance, initialized and ready for action.
<li><h3>toggleTracking(bTrack, bNotify)</h3>
Toggles change tracking in the editor. 
<h4>Parameters</h4>
<ul>
<li><code>bTrack</code> - boolean. If <code>undefined</code>, change tracking is toggled, otherwise it is set to this value
<li><code>bNotify</code> - boolean. If false, the <code>LITE.Events.TRACKING</code> event is not fired.
</ul>
<li><h3>toggleShow(bShow, bNotify)</h3>
Change the visibility of tracked changes. Visible changes are marked by a special style, otherwise insertions appear in their original format and deletions are hidden.
<h4>Parameters</h4>
<ul>
<li><code>bShow</code> - boolean. If <code>undefined</code>, change visibility is toggled, otherwise it is set to this value
<li><code>bNotify</code> - boolean. If false, the <code>LITE.Events.SHOW_HIDE</code> event is not fired.
</ul>

<li><h3>acceptAll</h3>
Accept all the pending changes.

<li><h3>rejectAll</h3>
Reject all the pending changes.

<li><h3>setUserInfo(info)</h3>
Sets the name and id of the current user. Each tracked change is associated with a user id. 
<h4>Parameters</h4>
<ul>
<li><code>info</code> - An object with two members - <code>id</code> and <code>name</code>
</ul>

</ul>
</div>

<h2>Events</h2>
<div class="doc"></div>
The LITE plugin events are listen in <code>lite_interface.js</code> under <code>LITE.Events</code>. The following events are fired by the LITE plugin instance, with the parameter in the <code>data</code> member of the event info:
<ul>
<li>INIT (parameters: lite, the LITE instance)<div>Fired each time <code>LITE</code> creates and initializes an instance of the <code>ICE</code> change tracker. This happens, e.g., when you switch back from <code>Source</code> mode to <code>Wysiwyg</code>.</div>
<li>ACCEPT_SOME (parameter : none)<div>Fired after some changes (but not all) were accepted. Out of the box, this is fired only when the <code>LITE.Commands.ACCEPT_ONE</code> command is executed.</div>
<li>ACCEPT_ALL (parameter : none)
<div>Fired after the all pending changes are accepted.</div> 
<li>REJECT_SOME (parameter : none)<div>Fired after some changes (but not all) were rejected. Out of the box, this is fired only when the <code>LITE.Commands.REJECT_ONE</code> command is executed.</div>
<li>REJECT_ALL (parameter : none)
<div>Fired after the all pending changes are rejected.</div> 
<li>SHOW_HIDE(parameter: show &lt;boolean&gt;)
<div>Fired after a change in the visibility of tracked changes.</div>
<li>TRACKING (parameter: tracking&lt;boolean&gt;)
<div>Fired after a change in the change tracking state.</div>

</ul>
</div>
<div id="footer">
<pre>Copyright 2013 LoopIndex, This file is part of the Track Changes plugin for CKEditor.

The track changes plugin is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License, version 2, as published by the Free Software Foundation.
This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.
You should have received a copy of the GNU General Public License along with this program as the file gpl-2.0.txt. If not, see http://www.gnu.org/licenses/old-licenses/gpl-2.0.txt
</pre>
</div>
</body>