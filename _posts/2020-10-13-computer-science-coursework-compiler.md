---
layout: post
title: Building a custom site generator for my computer science coursework
date: 2020-10-12T22:04:02.036Z
learned:
  - Ruby
  - Unix commands
---

For my computer science coursework (which will have a writeup later), I need to
write a report. This report is important as it is the main factor in influencing
how many marks I get.

I have a few requirements for how I'd like to edit this report:

  * Seperate files for seperate topics
  * A way to include parts from the specification at the top of each file (so I
    don't have to swap to the specification all the time)
  * Written in markdown format
  * Can edit using Vim (the only real editor)
 
I know, I'm picky.

Normally, I would use Jekyll for this. Sadly, there isn't a good way to bring
together multiple pages into one using Jekyll. Also, it has much more
functionality than I need.

I couldn't really find anything online that covered all of my requirements, so,
always happy to waste an afternoon, I decided to build my own solution.

The solution
------------

For each topic in the spec, I made a file. The file includes a header that looks
like this:

```
STARTSPEC
The specification goes here
ENDSPEC

Content that goes to the report
```

This way, I can include a part of the specification inside my file.

Now that the files are there. I wrote a compiler which takes all the files, puts
them into one big file, then parses it using markdown.

I had to reduce the header level of the headers in the files so that it would
fit in the master document, so I overrided the method of RedCarpet (the markdown
compiler) which renders headers.

Further, I added a table of contents, and numbered the headers (which I later
realised I could have done using CSS... oh well)

Finally, I added some styles which made the whole report look like an old RFC
document. Here's a screenshot:

![Screenshot](https://i.imgur.com/qoqaB6E.png)

[The full code is on Github](https://github.com/penguoir/computer-science-coursework)

here is the main part:

```ruby
require 'redcarpet'
require 'rouge'
require 'rouge/plugins/redcarpet'

# HTML Markdown renderer with SmartyPants
class Renderer < Redcarpet::Render::HTML
  include Redcarpet::Render::SmartyPants
  include Rouge::Plugins::Redcarpet

  @@header_counter = Array.new(6).fill(0)

  def header(text, header_level)
    # Everything after header level goes to 0
    @@header_counter.fill(0, (header_level)..)

    # Increment counter
    @@header_counter[header_level - 1] += 1

    # The list of numbers
    numbers = @@header_counter[0...header_level].join('.') + ' '

    # Create id for TOC
    id = text.downcase.gsub(/ /, '-').gsub(/[^A-Z|a-z|-]/, '')

    %Q(<h#{header_level + 1} id="#{id}">#{numbers + text}</h#{header_level + 1}>)
  end

  def footnote_ref(number)
    %Q([<a href="#fn#{number}">#{number}</a>])
  end
end

markdown = Redcarpet::Markdown.new(Renderer.new(:with_toc_data => true), {
  :no_intra_emphasis => true, # dont italic this_and_this
  :tables => true,
  :fenced_code_blocks => true,
  :autolink => true, # autolink links in <>
  :space_after_headers => true, # can't do #header
  :superscript => true, # 2^(nd)
  :footnotes => true, # blah blah [^1] ... [^1]: Footnote
  :with_toc_data => true
})

toc = Redcarpet::Markdown.new(Redcarpet::Render::HTML_TOC.new)

# All the documentation files
doc_files = Dir["*.md"]

# Sort the files by their title
doc_files.sort!

# Surround each element with ''
doc_files.map! { |f| "'#{f}'" }

# Append all files to .temp using ">"
`cat #{doc_files.join(' ')} > .temp`

# Delete specification lines using "sed"
`sed '/STARTSPEC/,/ENDSPEC/d' .temp -i` 

# Read the temp file
content = File.read(".temp")

# Read the CSS file
head = File.read("compile/head.html")

# Add the css, table of contents, and content to an HTML file
File.open('index.html', 'w') do |file|
  file << head
  file << %q(
    <h1 style="text-align: center; text-decoration: underline">
      Building a project-based learning management system
    </h1>
    <div style="display: flex; justify-content: space-between;">
      <p>
        Ori Marash [3024]
      </p>
      <p>
        King Alfred School [12254]<br/>
      </p>
    </div>
  ) 
  file << toc.render(content)
  file << markdown.render(content)
end

# Remove the temporary file
`trash .temp`

# Brag about it
puts "Created report with #{doc_files.length} files."
```

