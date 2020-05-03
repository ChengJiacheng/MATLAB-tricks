# MATLAB-tricks
## Syntax

```
(1/-2*sqrt(2)) != (1/(-2*sqrt(2)))
```

## Numerical Stability
### Softmax (2d)

Using exp(x) in MATLAB (e.g. exp(710)) will easily produce the overflow issue. To aviod this, a common trick is to subtract a constant from x. Notice that `softmax(x) =  softmax(x+c)`, where c is a constant. 
```
function y = softmax(x)
    % size(x) = [num_examples, num_classes]
    % size(y) = [num_examples, num_classes]
    x = x - repmat(max(x, [], 2), [1 size(x, 2)]);
    y = exp(x);
    y = y ./ (repmat(sum(y, 2), [1 size(x, 2)]));
end
```