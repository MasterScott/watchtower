Watchtower (v1.0.2)
===================
Chris Lane  
28 May 2012  
chris@chris-allen-lane.com  
http://chris-allen-lane.com  
http://twitter.com/#!/chrisallenlane


What it Does
------------
`Watchtower` is a [Static Code Analysis][1] tool designed to assist security
auditors who are tasked with performing manual code reviews. It is
platform- and language-agnostic.


View a Sample Report
--------------------
Is this Readme TLDR;? Download [/examples/sample_report.html][2],
and open it in a web-browser. It is an example report that was
generated by `Watchtower`.

Specifically, it's an HTML-formatted report that was run against the
popular [W3 Total Cache plugin][3] for Wordpress. If you spend a
few minutes playing with this report, and you should quickly gain a feel
for how `Watchtower` works.

### Notes about the sample report: ###
1. This is a big HTML file. I'd recommend opening it in a fast
browser like Google Chrome.
2. I chose to analyze W3 Total Cache because of its popularity. I'm not
implying that I believe it to be insecure.


Usage
-----
There are essentially two use-cases for `Watchtower`:

### Importing Scan Data into You Own Application ###
`Watchtower` can be configured (via command-line options) to output as
CSV, XML, or plain text:

```bash
# to output a CSV:
./watchtower -s /app/path/to/scan -o csv > /path/to/report/file.csv

# to output XML:
./watchtower -s /app/path/to/scan -o xml > /path/to/report/file.xml

# to output plain text:
./watchtower -s /app/path/to/scan -o txt
```

The CSV format is useful for importing scan data into a spreadsheet. The
XML format is useful for importing scan data into a custom application. TXT
is less likely to be useful, but it's there if you need it. (Know that you
can colorize plain-text output with the `--colorize` flag if you're
outputting directly to the terminal.)

### Outputting a Sortable HTML Report for Direct Review ###
In my opinion, `Watchtower`'s greatest feature is its ability to output
an HTML report. Such a report can be a tremendous timesaver when scanning
for a large number of signatures in large numbers of files.

This report is especially useful when auditing while running dual monitors,
such that you can have the report open on one monitor, and your preferred IDE
open on the other.

To generate the HTML report, run:
	
```bash
./watchtower -s /app/path/to/scan -o html > /path/to/report/file.html -p 'The Project Name'
```
    
Configuration
-------------
`Watchtower` has an extensive configuration file. (A large number of
configuration options have to do with formatting and content for HTML
reporting.) Please read the comments in the config file itself
(`.config.rb`) for more information.

Also, note that it's possible to specify which config file to use when
executing `Watchtower` via the `--config-file` option. This is useful
when you're working on several projects at once: rather than having
to repeatedly change values within the config file, you may simply create
alternate config files (`config-project-one.rb`, etc), and then specify
which to use upon execution.


Signature Specification
---------------------
Signatures live in `<project root>/signatures/` (by default), and are
organized by file-type, and then by group:

```ruby
$signatures[:filetype][:group]
```

As in:

```ruby
$signatures[:php][:dangerous_functions] = %w[signature signature signature ...]
```

Signature groupings will be respected when laying out an HTML report,
so creating thoughtful groupings can make reports more navigable.

If you're interested in creating a signature for a new file-type, run the
following in a terminal:

```bash
ack-grep --help-types
```

You'll need to use the `ack-grep` option name (symbolized) as the key
for your new signature. Thus `--java` will translate to `:java`.

Once you've determined the proper symbol to use for your new file-type, you
can create a new signatures file for that file-type. (I'd recommend that
you simply copy, paste, and modify an existing signatures file so you can
see the data structure you're trying to build as you work.)


Known Issues
------------
This program has only been tested on Ubuntu 11.04, and may not work on
other platforms. Additionally:

* `Watchtower` is currently unable to recognize ASP/X files, because `ack-grep`
  (upon which this program relies) itself cannot. I plan to fix
  this in the future.

* If you attempt to generate the same report more than once, you may find
  that the ordering of the elements contained therein will change. (Only
  the ordering of the information will change - the informational content
  itself will not.) This is because my Ruby-Fu is still weak, and I haven't
  yet figured out how to sort some of the nested hashes which the `VulnScanner`
  model assembles before outputting a report. I plan to address this issue
  very soon.


Roadmap
--------
I plan to make the following changes when I have the time:

* Fix the Known Issues
* Add more default signatures for different programming languages and frameworks
* Add `--after-context` and `--before-context` flags, as per `grep`
* Remove `ack-grep` as a dependency


Contributing
------------
I'm very interested in compiling signatures for popular 3rd-party frameworks.
(For an example, look at `/signatures/wordpress.rb`, which is
Wordpress-specific.) If you have expertise in a popular framework
(Joomla, Drupal, etc), and would like to contribute some signature files,
please let me know. 

Likewise, if you have expertise in a language which is not represented
among the signatures (web or otherwise), I invite you to share your
expertise.


Contact Me
----------
Questions, concerns, bug reports, and feature requests may be directed to
chris@chris-allen-lane.com. If you're able to express your thoughts in
144 characters or less ("It's great!", "It sucks!") you can also contact
me on [twitter][4].


License
-------
This product is licensed under the [GPL 3][5], and comes with absolutely
no warranty, expressed or implied. See the LICENSE file for more information.


[1]: http://en.wikipedia.org/wiki/Static_program_analysis
[2]: https://raw.github.com/chrisallenlane/watchtower/master/examples/sample_report.html
[3]: http://wordpress.org/extend/plugins/w3-total-cache/
[4]: http://twitter.com/#!/chrisallenlane
[5]: http://www.gnu.org/copyleft/gpl.html
