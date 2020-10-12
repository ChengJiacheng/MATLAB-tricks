# MATLAB-tricks
## Syntax

```
(1/-2*sqrt(2))  % -0.7071
(1/(-2*sqrt(2))) % -0.3536
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

## Figures
### Prevent Type 3 Fonts
The default font used by MATLAB figures is Helvetica, which is not allowed by some Journals/Conferences. My handling strategy is to use Times New Roman.
```
openfig('vis4.fig')
xlabel('$\mathbf{x}(1)$', 'Interpreter', 'latex', 'FontSize', 16, 'FontName', 'Times New Roman')
ylabel('$\mathbf{x}(2)$', 'Interpreter', 'latex', 'FontSize', 16, 'FontName', 'Times New Roman')

set(gca, 'FontName', 'Times New Roman')

hLegend = findobj(gcf, 'Type', 'Legend');
set(hLegend, 'FontName', 'Times New Roman')
saveas(gcf,'vis4','epsc')
```


# Python-tricks
## Check How Much Memory Is Being Used
```
import psutil, os
print(u'当前进程的内存使用：%.4f GB' % (psutil.Process(os.getpid()).memory_info().rss/ 1024 / 1024 / 1024))
```
## Display GPU Information
```
from pynvml import *
nvmlInit()
deviceCount = nvmlDeviceGetCount() #几块显卡
for i in range(deviceCount):
    handle = nvmlDeviceGetHandleByIndex(i)
    print ("Device", i, ":", nvmlDeviceGetName(handle)) #具体是什么显卡
```