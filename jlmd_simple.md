
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
