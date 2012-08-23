# Examples of generating interesting output from Julia

Here's an example of `dump()`'ing the type inheritance tree for
`Integer`'s. The output from dump is converted to Markdown lists
(basically, we need to add `"- "` at the front of each line).  

[[[Calculate]]]

```julia output=markdown
function dump_to_markdown(x)
    sm = memio(0,false)
    dump(sm, x)
    smt = takebuf_string(sm) 
    a = split(smt, "\n")
    function mysub(s::String, regex)
        r = match(r"^(\s*)(\S.*)$", s)
        !isequal(typeof(r), Nothing) ? "$(r.captures[1])- $(r.captures[2])" : ""
    end   
    mysub(sa::Array{String}, regex) = [mysub(s, regex) for s in sa]
    join(mysub(a, r"^(\s*)(\S.*)$"), "\n")
end
print(dump_to_markdown(Integer))
```

Now, let's see if we can turn a list into a tree. This uses a [Yahoo YUI
tree widget](http://developer.yahoo.com/yui/treeview/).

<!-- Individual YUI CSS files --> 
<link rel="stylesheet" type="text/css" href="http://yui.yahooapis.com/2.9.0/build/treeview/assets/skins/sam/treeview.css"> 
<!-- Individual YUI JS files --> 
<script type="text/javascript" src="http://yui.yahooapis.com/2.9.0/build/yahoo-dom-event/yahoo-dom-event.js"></script> 

```julia output=markdown
print(dump_to_markdown(Number))
println("")
# For some reason, the following javascript inclusion didn't work when
# placed above in the markdown portion.
println("<script type='text/javascript' src='http://yui.yahooapis.com/2.9.0/build/treeview/treeview-min.js'></script>")
println("<script type='text/javascript'>
var treeInit = function() {
    var tree = new YAHOO.widget.TreeView('jlmd_res_2');
    console.log(tree);
    tree.render();
};
setTimeout(treeInit, 300);
</script>
")
```
