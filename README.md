

#Pillar



##1\. Introduction

[Pillar](http://www.smalltalkhub.com/#!/~Pier/Pillar) is a markup syntax and associated tools to write and generate documentation and books\. Pillar is currently used to write the [Enterprise Pharo book](https://ci.inria.fr/pharo-contribution/job/PharoForTheEnterprise) and other projects \(see Section [1\.2](#pillarUSERS)\)\. The Pillar screenshot below \(see Figure [1\.1](#voyageDocExample)\) shows a part of the [Voyage documentation](https://github.com/SquareBracketAssociates/PharoForTheEnterprise-english/blob/master/Voyage/Voyage.pier) generated by Pillar\.

<a name="voyageDocExample"></a><figure><img src="figures/voyageDocExample-small.png" width="30%"></img><figcaption>An example Pillar output</figcaption></figure>

Pillar has many features:


-  simple markup syntax with references, tables, pictures, captions\.\.\.\);
-  export to HTML, LaTeX, Markdown and Pillar itself;
-  customization of the export through a dedicated [STON](http://smalltalkhub.com/#!/~SvenVanCaekenberghe/STON) configuration file;
-  support of templates using the [Mustache](http://smalltalkhub.com/#!/~NorbertHartl/Mustache) templating engine;
-  syntax\-highlighting of generated code blocks \(not yet in LaTeX\);
-  configurable numbering of section titles and figures;
-  \.\.\.

Pillar has also:
&nbsp;

-  a 5\-minutes tutorial
-  a documentation \(in progress\)
-  a good test coverage \(95% with more than a 1150 executed tests\)
-  a [continuous integration job](https://ci.inria.fr/pharo-contribution/job/Pillar)
-  a command\-line interface
-  several existing use cases \(see Section [1\.2](#pillarUSERS)\)
-  a [cheat sheet](http://www.cheatography.com/benjaminvanryseghem/cheat-sheets/pillar)


The contributors are:

&nbsp;

-  [Lukas Renggli](http://www.lukas-renggli.ch) created [Pier](http://piercms.com) a long time ago from which Pillar's document model, parser and many unit tests are coming;
-  [Benjamin van Ryseghem](http://benjamin.vanryseghem.com) did everything on the [Mardown](http://daringfireball.net/projects/markdown/) exporter and its [GitHub Markdown](https://help.github.com/articles/github-flavored-markdown) little brother;
-  [Benjamin van Ryseghem](http://benjamin.vanryseghem.com) also worked on the command\-line interface, the STON configuration interpreter and many other stuff;
-  [Guillermo Polito](http://playingwithobjects.wordpress.com) fixed a bug;
-  [Stéphane Ducasse](http://stephane.ducasse.free.fr) provided continuous feedback from the beginning of the project and got the original idea;
-  [Damien Cassou](http://damiencassou.seasidehosting.st) for everything else :\-\)\.



###1\.1\. Editors

These editors have dedicated plugins for Pillar:

&nbsp;

-  [Emacs](https://github.com/DamienCassou/pillar-mode)
-  [Vim](https://github.com/cdlm/vim-pillar)



###1\.2\. Pillar users

<a name="pillarUSERS"></a>
Pillar is used in several projects\. You can have a look at these projects for real use cases of Pillar\.

&nbsp;

-  [the Enterprise Pharo book](https://ci.inria.fr/pharo-contribution/job/PharoForTheEnterprise/)
-  [the Marina CMS](https://github.com/tide-framework/marina)
-  [the Pier CMS](http://piercms.com)
-  [the Pillar documentation itself](https://github.com/DamienCassou/pillar-documentation)
-  [the Spec documentation](https://github.com/SpecForPharo/documentation)
-  [the Pharo sound tutorial](https://github.com/xmessner/PharoSoundTutorial)



##2\. 5 minutes tutorial

In this section we give the basic steps to get you started with your first Pillar document and exports\. You first need to create a *base directory* inside which we will put all your text, configuration files, and Pillar itself\.



```bash
mkdir mydocument
cd mydocument
```





###2\.1\. Installing and exporting your first document

You first need to download Pillar\. For that, I recommend executing [this script](https://raw2.github.com/SquareBracketAssociates/PharoForTheEnterprise-english/master/download.sh) in the base directory\.

Then, you can check everything is working fine by creating a `first.pier` file with this content:



```smalltalk
!Hello World
```



And finally compiling it from a terminal \(see Section [6](#commandLineInterface) for more information about the command\-line interface\):



```bash
./pillar export --to='html' 'first.pier' > first.html
```



This should generate a `first.html` file you can open in a web browser\. This content of this file will be something like:



```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>No title</title>
    [...]
  </head>
  <body>
    <div class="container">
      <h1>Hello World</h1>
    </div>
    [...]
  </body>
</html>
```





###2\.2\. Configuring a document

As you can see, there is no title in the generated `first.html` file\. This is because we did not specify any\. To specify a title, we have to write a configuration file\. This configuration is typically named `pillar.conf` and is written in the [STON](http://smalltalkhub.com/#!/~SvenVanCaekenberghe/STON) format \(see Section [4](#configuring) for more information about the configuration\)\. Create your first `pillar.conf` file:



```ston
{
    "title" : "My first document while reading the 5 minutes Pillar tutorial"
}
```



When you compile using the same command line,



```bash
./pillar export --to=html first.pier > first.html
```



you should now get a web page with a title:



```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>My first document while reading the 5 minutes Pillar tutorial</title>
  </head>
```





###2\.3\. Exporting a different content using a template

If you want to tweak the content of the exported file, for example to reference your CSS or to add a footer, you need to create your own template \(see Section [5](#templating) for more information about templating\)\. You must write such template in its own file, e\.g\., `myhtml.template`:



```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>{{{title}}}</title>
  </head>
  <body>
    <div class="container">
      {{{content}}}
    </div>
    <footer>
      <p>Damien Cassou, 2014</p>
    </footer>
  </body>
</html>
```



Then, you need to reference this template from your configuration file\. So, edit your `pillar.conf` configuration file:



```ston
{
    "title" : "My first document while reading the 5 minutes Pillar tutorial",
    "configurations" : {
        "html" : {
            "template" : "myhtml.template",
            "outputType" : #html
        }
     }
}
```



Now, compile `first.pier`



```bash
./pillar export --to=html first.pier > first.html
```



to generate a `first.html` that will contain:



```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>My first document while reading the 5 minutes Pillar tutorial</title>
  </head>
  <body>
    <div class="container">
      <h1>Hello World</h1>
    </div>
    <footer>
      <p>Damien Cassou, 2014</p>
    </footer>
  </body>
</html>
```



This concludes our 5 minutes tutorial\.



##3\. Writing Pillar documents

<a name="writing"></a>
In this section we show how to write Pillar documents by presenting Pillar syntax\. You might want to have a look at the [cheat sheet](http://www.cheatography.com/benjaminvanryseghem/cheat-sheets/pillar) and even download and print it\.



###3\.1\. Chapters & Sections

A line starting with `!` becomes a chapter heading\. Use multiple `!` to create sections and subsections\.

<a name="chapterAndSections"></a>
To refer to a section or chapter, put an anchor \(equivalent to \\label\{chapterAndSections\} in Latex\) using the `@chapterAndSections` syntax on a *separate line*\. Then, when you want to link to it \(equivalent to \\ref\{chapterAndSections\} in Latex\), use the `*chapterAndSections*` syntax\. Anchors are invisible and links will be rendered as: [3\.1](#chapterAndSections)\.



###3\.2\. Paragraphs and framed paragraphs

An empty line starts a new paragraph\.

An annotated paragraph starts a line with `@@` followed by either `todo` or `note`\. For example,



```smalltalk
@@note this is a note annotation.
```



generates

&nbsp;


    Note: this is a note annotation.


And,



```smalltalk
@@todo this is a todo annotation
```



generates a todo annotation that is not visible in the output\.

&nbsp;


###3\.3\. Lists



####3\.3\.1\. Unordered lists



```smalltalk
-A block of lines,
-where each line starts with ==-==
-is transformed to a bulleted list, where each line is an entry.
```



generates

&nbsp;

- A block of lines,
- where each line starts with `-`
- is transformed to a bulleted list, where each line is an entry\.



####3\.3\.2\. Ordered lists



```smalltalk
#A block of lines,
#where each line starts with ==#==
#is transformed to an ordered list, where each line is an entry.
```



generates

&nbsp;

1. A block of lines,
2. where each line starts with `#`
3. is transformed to an ordered list, where each line is an entry\.



####3\.3\.3\. Definition lists

Definition lists \(*aka\.* description lists\) are lists with labels:



```smalltalk
;blue
:color of the sky
;red
:color of the fire
```



generates

<dl><dt>blue
</dt><dd>color of the sky</dd><dt>red
</dt><dd>color of the fire</dd></dl>


####3\.3\.4\. List nesting



```smalltalk
- Lists can also be nested.
-#Thus, a line starting with ==-#==
-#is an element of a bulleted list that is part of an ordered list.
```



generates

&nbsp;

-  Lists can also be nested\.
    1. Thus, a line starting with `-#`
    2. is an element of a bulleted list that is part of an ordered list\.




###3\.4\. Formatting
There is some sugar for font formatting:

&nbsp;

- To make something **bold**, write `""bold""`
- To make something *italic*, write `''italic''`
- To make something `monospaced`, write `==monospaced==`
- To make something ~~<del>strikethrough</del>~~, write `--strikethrough--`
- To make something <sub>subscript</sub>, write `@@subscript@@`
- To make something <sup>superscript</sup>, write `^^superscript^^`
- To make something underlined, write `__underlined__`



###3\.5\. Tables
To create a table, start off the lines with `|` and separate the elements with `|`s\. Each new line represents a new row of the table\. Add a single `!` to let the cell become a table heading\.



```smalltalk
|!Language |!Coolness
|Smalltalk | Hypra cool
|Java | baaad
```





| Language  | Coolness
| :---:| :---:
| Smalltalk  |  Hypra cool
| Java  |  baaad



The contents of cells can be aligned left, centered or aligned right by using `|{`, `||` or `|}` respectively\.



```smalltalk
||centered||centered||centered
|{ left |} right || center
```



generates



| centered | centered | centered
| :---:| :---:| :---:
|  left  |  right  |  center




###3\.6\. Links



####3\.6\.1\. Internal Links and Anchors

<a name="anchorName"></a>
To put an anchor \(equivalent to \\label in Latex\), use the `@anchorName` syntax on a *separate line*\. Then, when you want to link to it \(equivalent to \\ref in Latex\), use the `*anchorName*` syntax\. Anchors are invisible and links will be rendered as: [3\.6\.1](#anchorName)\.



####3\.6\.2\. External Links

To create links to externals resources, use the `*Pharo>http://pharo-project.org/*` syntax which is rendered as [Pharo](http://pharo-project.org/)\.



###3\.7\. Pictures

To include a picture, use the syntax `+caption>file://filename|parameters+`:



```smalltalk
+Caption of the picture>file://figures/pier-logo.png|width=50|label=pierLogo+
```



generates Figure [3\.1](#pierLogo) \(this reference has been generated using `*pierLogo*`\)\.

<a name="pierLogo"></a><figure><img src="figures/pier-logo.png" width="50%"></img><figcaption>This is the caption of the picture</figcaption></figure>



###3\.8\. Scripts

Use scripts when you want to add code blocks to your document\.




    \[\[\[
        foo bar
    \]\]\]


generates



```smalltalk
foo bar
```



If you want either a label \(to reference the script later\) or a caption \(to give a nice title to the script\), write the following:




    \[\[\[label=script1\|caption=My script that works\|language=Smalltalk
        self foo bar
    \]\]\]


which produces



<a name="script1"></a>**My script that works
**

```smalltalk
self foo bar
```



This script can then be referenced with `*script1*` \(produces [3\.1](#script1)\)\. Specifying the language \(here `Smalltalk`\) will give you syntax highlighting\.



###3\.9\. Raw

If you want to include raw text into a page you must enclose it in `{{{` and `}}}`, otherwise Pier ensures that text appears as you type it\.

A good practice is to always specify for which kind of export the raw text must be outputted by starting the block with `{{{latex:` or `{{{html:` \(for now only LaTeX, HTML and Markdown are supported\)\. For example, the following shows a formula, either using LaTeX or an image depending on the kind of export\.




    \{\{\{latex:
    \\begin\{equation\}
      \\label\{eq:1\}
      \\frac\{1\+\\sqrt\{2\}\}\{2\}
    \\end\{equation\}
    \}\}\}
    \{\{\{html:
    \(1\+sqrt\(2\)\) / 2
    \}\}\}



This results in




**Take care:** avoid terminating the verbatim text with a `}` as
this will confuse the parser\. So, don't write ~~<del>`{{{\begin{scriptsize}}}}`</del>~~ but `{{{\begin{scriptsize} }}}` instead\.



###3\.10\. Preformatted \(less used\)

To create a preformatted block, begin each line with `=`\. A preformatted block uses equally spaced text so that spacing is preserved\.




    = this is preformatted text
    = this line as well




###3\.11\. Commented lines

Lines that start with a `%` are considered comments and will not be rendered in the resulting document\.



##4\.  Configuring your output

<a name="configuring"></a>
In this section we show how to configure the export from the Pillar files\.



###4\.1\.  Configuration file

Pillar exporting mechanism can be configured using [STON](http://smalltalkhub.com/#!/~SvenVanCaekenberghe/STON), a lightweight, text\-based, human\-readable data interchange format \(similar to the popular [JSON](http://www.json.org)\.



###4\.2\.  Configuration parameters



####4\.2\.1\.  baseDirectory
<a name="sec:confParam:baseDirectory"></a>Indicate where to look for files with a non\-absolute path\.

<dl><dt>Default value
</dt><dd>The current working directory.</dd></dl>


####4\.2\.2\.  configurations
<a name="sec:confParam:configurations"></a>Each configuration can define several sub configurations, each of which inherits the properties of its parent\.

<dl><dt>Default value
</dt><dd>A dictionary of default configurations from the exporters.</dd></dl>


####4\.2\.3\.  defaultScriptLanguage
<a name="sec:confParam:defaultScriptLanguage"></a>Indicate the \(programming\) language in scripts when none is specified\.
This language is used for syntax highlighting\.

<dl><dt>Default value
</dt><dd>None</dd></dl>


####4\.2\.4\.  headingLevelOffset
<a name="sec:confParam:headingLevelOffset"></a>Indicate how to convert from the level of a Pillar heading to the level of heading in your exported document\.
For example, a `headingLevelOffset` of 3 converts a 1st level Pillar heading to an `<h3>` in HTML\.

<dl><dt>Default value
</dt><dd>`0`</dd></dl>


####4\.2\.5\.  inputFiles
<a name="sec:confParam:inputFiles"></a>List the Pillar files that must be exported\.

<dl><dt>Default value
</dt><dd>None</dd></dl>


####4\.2\.6\.  newLine
<a name="sec:confParam:newLine"></a>The string that separates lines in the exported document\.
This is often either LF or CR\+LF but any string is possible\.

<dl><dt>Default value
</dt><dd>Depend on the operating system.</dd></dl>


####4\.2\.7\.  outputFile
<a name="sec:confParam:outputFile"></a>Indicate in which file to export input files\.

<dl><dt>Default value
</dt><dd>Standard output.</dd></dl>


####4\.2\.8\.  outputType
<a name="sec:confParam:outputType"></a>Indicate the kind of output desired\.

<dl><dt>Default value
</dt><dd>None</dd></dl>


####4\.2\.9\.  separateOutputFiles
<a name="sec:confParam:separateOutputFiles"></a>Indicate whether each input file must be exported separately or not\.

<dl><dt>Default value
</dt><dd>`false`</dd></dl>


####4\.2\.10\.  startNumberingAtHeadingLevel
<a name="sec:confParam:startNumberingAtHeadingLevel"></a>Indicate the level of Pillar heading that is going to be numbered with top level numbers\.
E\.g\., a `startNumberingAtHeadingLevel` of value `2` indicates that Pillar heading of level 1 are not numbered\.

<dl><dt>Default value
</dt><dd>`2`</dd></dl>


####4\.2\.11\.  stopNumberingAtHeadingLevel
<a name="sec:confParam:stopNumberingAtHeadingLevel"></a>Indicate the level of Pillar heading at which Pillar stops numbering\.
E\.g\., a `stopNumberingAtHeadingLevel` of value `4` indicates that Pillar heading of level 4 and more are not numbered\.

<dl><dt>Default value
</dt><dd>Infinity (never stop numbering).</dd></dl>


####4\.2\.12\.  template
<a name="sec:confParam:template"></a>Indicate the overall structure of the exported documents\.

<dl><dt>Default value
</dt><dd>`'{{{content}}}'` \(output the document as is, without any preamble or postamble\)\.</dd></dl>


####4\.2\.13\.  title
<a name="sec:confParam:title"></a>Indicate the main title of the document\.

<dl><dt>Default value
</dt><dd>`'No title'`</dd></dl>


####4\.2\.14\.  verbose
<a name="sec:confParam:verbose"></a>Indicate whether Pillar should write a verbose log when exporting\.

<dl><dt>Default value
</dt><dd>`false`</dd></dl>



##5\.  Templating
<a name="templating"></a>
Pillar comes with the [Mustache](http://smalltalkhub.com/#!/~NorbertHartl/Mustache) templating engine\. This means you can specify a preamble and postamble for your document\. Here is an example \(bootrap\-based\) HTML template using Mustache:



```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>{{{title}}}</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="http://netdna.bootstrapcdn.com/bootstrap/3.0.3/css/bootstrap.min.css" rel="stylesheet">
    <link href="http://netdna.bootstrapcdn.com/bootstrap/3.0.3/css/bootstrap-theme.min.css" rel="stylesheet">
    <link rel="stylesheet" href="http://yandex.st/highlightjs/8.0/styles/default.min.css">
    <script src="http://yandex.st/highlightjs/8.0/highlight.min.js"></script>

    <!--[if lt IE 9]>
      <script src="https://oss.maxcdn.com/libs/html5shiv/3.7.0/html5shiv.js"></script>
      <script src="https://oss.maxcdn.com/libs/respond.js/1.3.0/respond.min.js"></script>
    <![endif]-->
  </head>
  <body>

    <div class="container">
      {{{content}}}
    </div>

    <script src="https://code.jquery.com/jquery.js"></script>
    <script src="http://netdna.bootstrapcdn.com/bootstrap/3.0.3/js/bootstrap.min.js"></script>
    <script>hljs.initHighlightingOnLoad();</script>
  </body>
</html>
```



In this example, we can see the use of `{{{title}}}` and `{{{content}}}` to refer to the title of the document and its actual content \(the one exported from Pillar\)\. You have to put such a template in a dedicated file \(named `chapter.html.template` for example\) and reference this file from the template configuration parameter \(see [4\.2\.12](#sec:confParam:template)\)\.


##6\.  Command\-line interface
<a name="commandLineInterface"></a>
In this section we show how to use the `pillar` command\-line interface\.



```bash
$ ./pillar export --to=latex PharoSound.pier > PharoSound.tex
```


