
# Julia Markdown pages (Julia MD or JLMD)

Julia Markdown pages are meant to be an easy way to make simple web
interfaces or workbooks using [Julia](http://www.julialang.org).
The target audience is someone who knows Julia and wants to make web
applications probably for use on an intranet.

Markdown is an easy way to make web pages, and Julia already works
well through the web using the web REPL. Julia code blocks become
"live" when Julia Markdown pages are served through the Julia
webserver. Form elements entered using a Markdown extension for forms
are also converted to Julia variables.

Here is an example of a Julia code input section. When the page is
calculated (hitting the `calculate` button or the F9 key), the output
of each Julia section will appear below the input. Here's some example
Markdown:

    ## Simple function plotter
    
    alpha = ___(3.0) 
    
    [[[Calculate]]]
    
    ```julia output=markdown 
    println("## Results")
    ```
    
    ```julia
    f(x, k) = 20*sin(k * x) ./ x
    x = linspace(-5.,5,500)
    plot(x, f(x, float(alpha)))
    ```

When run, it will look like this in a browser:

![jlmd screen capture](https://raw.github.com/tshort/JuliaMarkdown/master/jlmd_screenshot.png)

In the Julia block header, you can specify the result type as
`markdown` for Markdown output (also useful for HTML, since Markdown
files can contain HTML). `output` can also be `"none"` to suppress
output. 

In the example above, a text entry box is specified with `alpha` =
`___(3.0)`. In Julia, `alpha` is assigned to the value entered in the
text box (a string). The default value is "3.0".

You add a `calculate` button to a Markdown file by inserting
[[[`Calculate`]]].


## Installing / Using

Copy all of these files to `julia_dir/usr/lib/julia/website/` where `julia_dir`
is the directory where Julia is installed. These files may be linked
to `julia_dir/ui/website/`. 

The following files are mandatory: jlmd.js, jlmd.html, jlmd.css, and
jlmd_showdown.js.

Then, start the Julia webserver. To load a Julia MD file use a URL
like the following:

    http://localhost:2000/jlmd.htm?jlmd-filename.md

Adjust the `jlmd-filename.md` part for different files (they don't
have to have "jlmd" in the filename). These files must all be in the
julia/usr/lib/julia/website/ directory (possibly linked from
julia/ui/website/).


## Examples

* jlmd_example1.md --
  [live local link](http://localhost:2000/jlmd.htm?jlmd_example1.md), [raw](https://github.com/tshort/JuliaMarkdown/blob/master/jlmd_example1.md)
  -- Broad overview.

* jlmd_example2.md --
  [live local link](http://localhost:2000/jlmd.htm?jlmd_example2.md), [raw](https://github.com/tshort/JuliaMarkdown/blob/master/jlmd_example2.md)
  -- Example function calculator. 

* jlmd_example3.md --
  [live local link](http://localhost:2000/jlmd.htm?jlmd_example3.md), [raw](https://github.com/tshort/JuliaMarkdown/blob/master/jlmd_example3.md)
  -- More form examples. 

* jlmd_example4.md --
  [live local link](http://localhost:2000/jlmd.htm?jlmd_example4.md), [raw](https://github.com/tshort/JuliaMarkdown/blob/master/jlmd_example4.md)
  -- Interesting output from Julia.


## Inspiration / Ideas

* [Rpad](http://cran.r-project.org/web/packages/Rpad/index.html), [live link](http://144.58.243.47/Rpad/)

* [R Markdown](http://rstudio.org/docs/authoring/using_markdown),
  [Rpubs hosting](http://rpubs.com/)

* [IPython notebook](http://ipython.org/ipython-doc/dev/interactive/htmlnotebook.html)


## How it works

Most of the infrastructure for this was already in place for the web
REPL. 

*Markdown conversion* -- The Markdown is converted to HTML using a
modified version of Showdown. The modifications combine a github
version of [Showdown](https://github.com/coreyti/showdown/) with an
[extension for form elements](https://github.com/brikis98/wmd) and
some small additions to support the Julia MD extensions like
`[[[Calculate]]]`. Each Julia code block is wrapped with a `DIV` that
has a placeholder for results. 

*Server connection and Javascript processing* -- Communication with
the server is handled using the same infrastructure as the web REPL
(which looks like it's mostly thanks to Stephan Boyer). The file that
does most of the work is `julia/ui/website/jlmd.js`. This is adapted
from `julia/ui/website/repl.js`. Plotting is handled with D3, just
like the web REPL. One difference is that results from Julia need to
be plugged into appropriate places in the HTML file. That's done by
making a separate user\_name (and user\_id) for each Julia code block.
That way, when the appropriate evaluated results are returned from the
server, the results are plugged into the right spot. Each webpage has
it's own session. 

## Current status

Currently, things are a bit rough, but pages calculate pretty well.
The web page collects three main types of output from the webserver:
`MSG_OUTPUT_OTHER`, `MSG_OUTPUT_EVAL_RESULT`, and `MSG_OUTPUT_PLOT`.
Of these, only `MSG_OUTPUT_EVAL_RESULT` has an indicator of the
calling location. For the others, the active location is tracked. It
mostly seems to work but seems a bit kludgy. Because each webpage has
its own session, the startup time is a bit slow waiting for the Julia
process to load. It would be faster if the Julia web server always had
a spare process ready to hand off to the next session request.

## Issues

* If JLMD connections are made from more than one tab in a browser,
  things will get messed up. The web REPL has the same problem. Both
  are from the use of cookies to manage sessions. See Julia
  [issue #1197](https://github.com/JuliaLang/julia/issues/1197).

* It doesn't seem to work in the Windows port of Julia
  [17f50ea4e0](https://github.com/downloads/JuliaLang/julia/julia-17f50ea4e0-WINNT-i686.tar.gz).

## Future

I'm really waiting for the Julia compiler to arrive. If we have that,
I think we can use
[Emscripten](https://github.com/kripken/emscripten/wiki) to compile
Julia Markdown pages to HTML and Javascript.
[PNaCl](http://www.chromium.org/nativeclient/pnacl/building-and-testing-portable-native-client)
is another possibility. Then, Julia Markdown pages could be served
from static locations like Github.


