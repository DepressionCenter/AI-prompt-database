![Depression Center Logo](https://github.com/DepressionCenter/.github/blob/main/images/EFDCLogo_375w.png "depressioncenter.org")


# **AI Prompts Database**
#### *__Real-world examples of UMGPT, Maizey and other AI prompts from the University of Michigan.__*

<br />

## Apply University of Michigan template to a webpage

### Contributors:
+ Gabriel Mongefranco ([@gabrielmongefranco](https://github.com/gabrielmongefranco)), Eisenberg Family Depression Center

### Description
Apply the current __umich.edu__ template to a webpage, by creating in-line styles and re-formatting the given HTML code.

### Template
**For: GPT**  <br />
**Prompt:**
<pre><code>
Make the following web page more visually appealing, looking more like a page that is part of umich.edu with a similar color theme and a umich logo:  
<var>[INPUT]</var>.
</code></pre>
<br />


### Examples

Enter description of the example here: (or delete this line)<br />
<kbd>
Make the following web page more visually appealing, looking more like a page that is part of umich.edu with a similar color theme and a umich logo.  
&lt;html&gt;  
&lt;head&gt;  
&lt;title&gt;U-M Mobile Data Dictionaries&lt;/title&gt;  
&lt;/head&gt;  
&lt;body&gt;  
&lt;h1&gt;U-M Mobile Data Dictionaries&lt;/h1&gt;  
&lt;h2&gt;Summary&lt;/h2&gt;  
&lt;p&gt;This is a listing of data dictionaries from U-M studies that use mobile data. These data dictionaries have been produced automatically using SchemaSpy.&lt;/p&gt;  
&lt;h2&gt;Available Data Dictionaries&lt;/h2&gt;  
&lt;p&gt;  
&lt;ul&gt;  
&lt;li&gt;&lt;a href=&quot;IHS_2023/index.html&quot;&gt;IHS Study (2023 Cohort)&lt;/a&gt;&lt;/li&gt;  
&lt;li&gt;&lt;a href=&quot;PROMPT/index.html&quot;&gt;PROMPT Study&lt;/a&gt;&lt;/li&gt;  
&lt;li&gt;&lt;a href=&quot;USAGESTATS/index.html&quot;&gt;MTC Usage Stats&lt;/a&gt;&lt;/li&gt;  
&lt;/ul&gt;  
&lt;/p&gt;  
&lt;br /&gt;  
&lt;hr /&gt;  
&lt;p&gt;For more information, please refer to the knowledge base at: &lt;a href=&quot;https://michmed.org/efdc-kb&quot;&gt;https://michmed.org/efdc-kb&lt;/a&gt;&lt;/p&gt;  
&lt;/body&gt;  
&lt;/html&gt;  
</kbd><br /><br />
![image](https://github.com/DepressionCenter/AI-prompt-database/assets/42566461/8fe6a5c0-be45-4898-98bd-15419f35a0dc)
<br /><br />
<samp>
&lt;!DOCTYPE html&gt;  
&lt;html lang=&quot;en&quot;&gt;  
&lt;head&gt;  
    &lt;meta charset=&quot;UTF-8&quot;&gt;  
    &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1.0&quot;&gt;  
	&lt;meta http-equiv=&quot;Refresh&quot; content=&quot;3; url='https:michmed.org/efdc-kb'&quot; /&gt;  
    &lt;style&gt;  
        body {  
            font-family: 'Helvetica', sans-serif;  
            line-height: 1.6;  
            background-color: #f3f3f3; /* University of Michigan off-white */  
            color: #00274c; /* University of Michigan dark blue */  
            margin: 0;  
            padding: 0;  
        }  
        .container {  
            width: 80%;  
            margin: 0 auto;  
            overflow: hidden;  
        }  
        .umich-logo {  
            height: 50px;  
            margin-top: 20px;  
        }  
        h1 {  
            color: #ffcb05; /* University of Michigan maize */  
        }  
        a {  
            color: #00274c; /* University of Michigan dark blue */  
        }  
        p {  
            margin-bottom: 20px;  
        }  
        .footer {  
            font-size: 0.8em;  
        }  
        .redirection-notice {  
            margin-top: 50px;  
            background-color: #e5e5e5; /* Light grey background for emphasis */  
            padding: 20px;  
            border-left: 4px solid #00274c; /* University of Michigan dark blue */  
        }  
    &lt;/style&gt;  
    &lt;!-- Page Title --&gt;  
    &lt;title&gt;EFDC Automation and APIs Server&lt;/title&gt;  
    &lt;!-- Redirection Script --&gt;  
    &lt;script type=&quot;text/javascript&quot;&gt;  
        window.setTimeout(()=&gt;{ window.location.href=&quot;https://michmed.org/efdc-kb&quot;; }, 3500);  
    &lt;/script&gt;  
&lt;/head&gt;  
&lt;body&gt;  
    &lt;div class=&quot;container&quot;&gt;  
        &lt;img src=&quot;https://github.com/DepressionCenter/.github/raw/main/images/EFDCLogo_375w.png&quot; alt=&quot;U-M Eisenberg Family Depression Center Logo&quot; class=&quot;umich-logo&quot;&gt;  
        &lt;!-- Main Content --&gt;  
        &lt;h1&gt;EFDC Automation and APIs&lt;/h1&gt;  
        &lt;p&gt;Welcome to the Automation and API server from the &lt;a href=&quot;https://depressioncenter.org&quot;&gt;Eisenberg Family Depression Center.&lt;/a&gt;&lt;/p&gt;  
		&lt;p&gt;&amp;nbsp;&lt;/p&gt;  
        &lt;div class=&quot;redirection-notice&quot;&gt;  
            &lt;p&gt;If you are not automatically redirected, please visit our knowledge base for usage instructions at: &lt;a href=&quot;https://michmed.org/efdc-kb&quot;&gt; https://michmed.org/efdc-kb &lt;/a&gt;.&lt;/p&gt;  
        &lt;/div&gt;  
		&lt;p&gt;&amp;nbsp;&lt;/p&gt;  

        &lt;!-- Footer --&gt;  
        &lt;div class=&quot;footer&quot;&gt;  
            &lt;strong&gt;Eisenberg Family Depression Center&lt;/strong&gt;&lt;br&gt;  
            &lt;a href=&quot;https://depressioncenter.org&quot;&gt;https://depressioncenter.org&lt;/a&gt;&lt;br&gt;  
            &lt;small&gt;&copy; 2023-2024 Regents of the University of Michigan&lt;/small&gt;  
        &lt;/div&gt;  
    &lt;/div&gt;  
&lt;/body&gt;  
&lt;/html&gt;  
<br /><br />
![image](https://github.com/DepressionCenter/AI-prompt-database/assets/42566461/c24965ee-905f-4ab8-bbcf-bde74f7745d2)  

 

</samp><br /><br />

----

Copyright © 2024 The Regents of the University of Michigan
