<h1>GAP (Get All Params) by @xnl_h4ck3r</h1>
<hr><br>
<b>This is an evolution of the original getAllParams extension for Burp Suite. Not only does it find more potential parameters for you to investigate, but it also finds potential links to try these parameters on. This is to be used instead of the original getAllParams extension if you want to make use of the additional link functionality. 
This code is far from perfect, but any constructive criticism is very much welcome! I hope this tool helps you.
</b>
<br>
<h3>Acknowledgments:</h3> 
Respect and thanks go to @HolyBugx for help with ideas, testing and patience!<br>
A shout out to Gerben Javado and his amazing tool <b>Link Finder</b> who's regular expression (regex) provided the starting point for the Link mode in GAP.

<h1>How to Install</h1>
<ol>
<li>Get <code>GAP.py</code> (https://github.com/xnl-h4ck3r/burp-extensions/blob/main/GAP.py)</li>
<li>Point Burp Suite to the Jython .jar file in <b>Extender > Options > Python Environment</b></li>
<li>On <b>Extensions</b> tab, click <b>Add</b></li>
<li>Set <b>Extension type</b> to <b>Python</b> and select the <code>GAP.py</code> file</li>
<li>Click <b>Next</b> and you're good to go <b>&#129304;</b></i>
</ol>
<h1>How to Run</h1>
On the <b>Target -> Site map</b> tab of Burp you can see select a specific host, a selection of hosts (holding down <i>Ctrl</i> or <i>Shift</i>), or all hosts (using <i>Ctrl-A</i>). 
Once the required hosts are selected, right click and select <b>Extensions -> GAP</b> to run the tool.
Go to the <b>GAP</b> tab and see the results. What gets returned will depend on the options selected, and these will all be described below.
For very large projects (and depending on what options were selected), it can sometimes take GAP a little while to run. If for some reason it hasn't completed and you want to cancel the current run to change options for example, you can do this by pressing the <b>CANCEL GAP</b> button.
If you try running GAP again while it is still running, it will CANCEL the current run before starting the new one.

<h1>GAP Mode</h1>
There are 2 different modes for GAP, <b>Parameters</b> and <b>Links</b>. They can either be run separately, or together, depending on what you select.
What each mode does will be explained below, but if you don't need both enabled then unselecting one can use less memory and get results back quicker.

<h1>Parameters Mode</h1>

When the GAP Mode of Parameters is selected then GAP will try to find as many potential parameters based the following options:
<h2>Request Parameters</h2>
These are parameters that Burp itself identifies from HTTP requests and are part of the Burp Extender API IParameter interface.
<ul>
<li><b>Query string params</b> - PARAM_URL; a parameter within the URL query string</li>
<li><b>Message body params</b> - PARAM_BODY; a parameter within the message body </li>
<li><b>Param attribute within a multi-part message body</b> - PARAM_MULTIPART_ATTR; the value of a parameter attribute within a multi-part message body (such as the name of an uploaded file)</li>
<li><b>JSON params</b> - PARAM_JSON; an item of data within a JSON structure</li>
<li><b>Cookie names</b> - PARAM_COOKIE; an HTTP cookie name</li>
<li><b>Items of data within an XML structure</b> - PARAM_XML</li>
<li><b>Value of tag attribute within XML structure</b> - PARAM_XML_ATTR</li>
</ul>

<h2>Response Parameters</h2>

These are potential parameters that can be found in the HTTP responses. These are identified by GAP itself rather than through the Burp Extender API.
<ul>
<li><b>JSON params</b> - if the response has a MIME type of JSON then the Key names will be retrieved</li>
<li><b>Value of tag attributes within XML structure</b> - if the response has a MIME type of XML then the XML attributes are retrieved</li>
<li><b>Name and Id attributes of HTML input fields</b> - if the response has a MIME type of HTML then the value of the NAME and ID attributes of any INPUT tags are retrieved</li>
<li><b>Javascript variables and constants</b> - javascript variables set with <code>var</code>, <code>let</code> or <code>const</code> are retrieved. <b>NOTE: Improvements are needed to retrieve more variables as there are many ways that these can be declared and difficult to retrieve all from regex.</b></li>
<li><b>Name attribute of Meta tags</b> - if the response has a MIME type of HTML then the NAME attribute of any META tags are retrieved</li>
<li><b>Params from links found</b> - THIS OPTION IS ONLY ENABLED IF LINKS MODE IS ALSO USED. Any URL query string parameters in potential Links found will be retrieved, only if they are clearly in scope, or there is just a path and no way of determining if it is in scope.</li>
</ul>

<h2>Output Options</h2>
The options under this section of the tool that specifically relate to Parameter mode are:
<ul>
<li><b>Include the list of common params in list (e.g. used for redirects)?</b> - Common parameter names are often used across targets, mainly for redirects. These can be included in the potential parameter list by checking this option. The list is stored in constant <code>COMMON_PARAMS</code>. <b>NOTE: Some values are the same, but different case in places as these will be treated differently</b></li>
<li><b>Build concatenated query string with param value</b> - If checked, the potential parameters found will be built into a query string, each with the value given in the text box (defaults to XNLV) followed by a unique number.</li>
<li><b>Include URL path words in parameter list?</b> - The words in the response URL path are included as potential parameters if the URL is in scope.</li>
</ul>

<h1>Links Mode</h1>

When the GAP Mode of Links is selected then GAP will try to find possible links based on the following. Also, only requests of a certain <i>Content-Type</i> are checked for potential links. This is determined by the constant <code>CONTENTTYPE_EXCLUSIONS</code> in the code (these are types such as images, video, audio, fonts, etc.)
<ul>
<li><b>Include site map endpoints in link list?</b> - This will include endpoints from the Burp Site map in the potential Link list, if they are in scope.</li>
<li><b>Link exclusions</b> - The field contains a comma separated list of values. If any of these values exists in a potential link found, then it will be excluded from the final list. There is a initial default list determined by the <code>DEFAULT_EXCLUSIONS</code> constant, but you can change this and save your settings.</li>
</ul>

<h1>GAP Output</h1>
Below is an explanation of the output given when GAP has completed running.

<h2>Potential Parameters</h2>
<ul>
<li><b>Potential parameters found</b> - This text are will show all unique potential parameters, one per line.</li>
<li><b>The latest generated query string of all parameters</b> - This is a generated URL query string combining all potential parameters found, all with the value specified in the Output option <b>Build concatenated query string with param value</b>, followed by a sequential number. This can be added to an endpoint and the response searched for the param value (e.g. XNLV by default) and if reflected the number will indicate which parameter was reflected.</li>
</ul>
<h2>Potential Links</h2>
<ul>
<li><b>Potential links found</b> - This text area will show potential links found. Without any of the other options described below selected, all unique endpoints found are displayed, one per line.</li>
<li><b>Show origin endpoint</b> - If this feature is ticked, the potential link will be followed by the HTTP request endpoint (in square brackets) that the link was found in. A link could have been found in more than one request, so this view can show duplicate links, one per origin endpoint.</li>
<li><b>In scope only</b> - If this feature is ticked, and the potential links contain a host, then this link will be checked against the Burp Target Scope. If it is not in scope then the link will be removed from the output. <b>NOTE: This does not take any Burp <i>Exclude from scope</i> entries into account. Also, if it is not possible to determine the scope (e.g. it may just be a path without a host) then it will be included as in scope to avoid omitting anything potentially useful.</b></li>
<li><b>Link filter</b> - any value entered in the Filter input field followed by <b>ENTER</b> or pressing <b>Apply filter</b> will determine which links will be displayed. This can depend on the values of the following two options:</li>
<li><b>Negative match</b> - If selected, any link containing the Filter text will NOT be displayed. If unselected, then only links containing the filter will be displayed.</li>
<li><b>Case sensitive</b> - If selected, the value is the Filter input field will be case sensitive when determining which Links to display.</li> 	
</ul>
The filter is something that is applied after GAP has run. It allows you to look for specific things when there are many results. For example, enter <code>.js</code> to only show the links to javascript files. As soon as you clear the filter, the original results are redisplayed.<br>
<br>
An additional feature of GAP is to automatically include links of valid <code>.js.map</code> (javascript source map) files. These are identified by responses that contain the <code>//# sourceMappingURL</code> line, or have a HTTP header of <code>SourceMap</code> or <code>X-SourceMap</code>.<br>
<br>
To find links, a complex regex is used to look for different formats and contexts for potential links and files. This regex was initially based on the one used in <b>Link Finder</b> by Gerben Javado, but has been evolved to try and identify more with minimal false positives. 

<h1>Output File Option</h1>
The results of GAP can be written to files as well as being displayed in the tool.
<ul>
<li><b>Auto save output to directory</b> - If this option is checked then when GAP completes a run, a file will be created with the potential parameters (if Parameters mode is selected) and with potential links (if Links mode is selected). These files will be created in the specified directory. If the directory is invalid then the users home directory will be used. 
<li><b>Choose...</b> - the button can be used to select the required directory to store output files. </li>
</ul>
The file names will be named in the following format: <code>[host]_GAP_params.txt</code> and <code>[host]_GAP_links.txt</code> where <code>[host]</code> is the first selected host name in the site map that GAP was run against.

<h1>GAP Settings</h1>
When GAP is first started, it will start with default settings. 
Any changes made to the configuration settings of GAP can be saved for future use by clicking the <b>Save options</b> button.
If for any reason you want to revert to the default configuration options, you can click the <b>Restore defaults</b> button.

<h1>Troubleshooting and Feedback</h1>
If you have any problems with GAP, you can report an issue on Github. Before you report an issue, please look at the <b>Extender -> Extensions</b> tab in Burp, click on the GAP extension in the list and include details of any output displayed on the <b>Errors</b> tab with your issue. If you know of a parameter or link that you believe GAP should/shouldn't have identified then please provide as much info as possible, e.g. the options you had selected, the relevant endpoint, etc. <br>
<br>
Thank you for trying out GAP!<p>
@xnl-h4ck3r
<b>&#129304;</b>
