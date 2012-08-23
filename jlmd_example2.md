<style type="text/css">
div.juliablock pre {visibility:hidden;height:0px}
.juliaresult {background-color:transparent;}
</style>

# Example function calculator

This example demonstrates the following:

* Hiding code blocks
* Running a Julia code block on page load
* Making Julia results blend in with the page
* Dynamically creating a form element with Julia

The following is a form created by Julia. This is only run upon
initiation. Pick from the following equations for g1 and g2. Upon
calculation, g2 will be plotted against g1.

    f1(x) = sin(1./x)
    f2(x) = 20*sin(3x) ./ x
    f3(x) = (1 - 4x - x.^3.0/17) .* sin(x.^2.)

The following block is run upon page load (`run=init`), and the form
is specified using Markdown syntax. 

```julia output=markdown run=init
f1(x) = sin(1./x)
f2(x) = 20*sin(3x) ./ x
f3(x) = (1 - 4x - x.^3.0/17) .* sin(x.^2.)
funlist = [identity,f1,f2,f3]
# We don't really have to generate this dynamically, but this
# can be useful in other situations.
println("g1 = {1 -> x, 2 -> f1, 3 -> f2, 4 -> f3}")
println("g2 = {1 -> x, (2 -> f1), 3 -> f2, 4 -> f3}")
```

[[[Calculate]]]

```julia output=markdown 
println("## Results")
```
```julia
x = linspace(-5, 5., 500)
plot(funlist[int(g1)](x), funlist[int(g2)](x))
```
