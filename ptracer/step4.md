# Filter

Ptracer allows elaborate syscall filtering via the filter argument, we can upgrade the previous program with the highlighted parts:
```python{13-18, 20}
import traceback
import ptracer
import re

def callback(syscall):
    print('{}({}) -> {}'.format(
        syscall.name,
        ', '.join(repr(arg.value) for arg in syscall.args),
        syscall.result))
    print('Traceback: ')
    print(''.join(traceback.format_list(syscall.traceback)))

flt = [
    ptracer.SysCallPattern(
        name='openat',
        result=lambda res: res.value > 0
    )
]

with ptracer.context(callback, filter=flt):
    open('/tmp/null', 'wb')
```
Let's create a new file for the new script
```
touch PtracerFiltering.py
```{{exec}}
And let's write there the new code
```
echo "import traceback
import ptracer

def callback(syscall):
    print('{}({}) -> {}'.format(
        syscall.name,
        ', '.join(repr(arg.value) for arg in syscall.args),
        syscall.result))
    print('Traceback: ')
    print(''.join(traceback.format_list(syscall.traceback)))

flt = [
    ptracer.SysCallPattern(
        name='openat',
        result=lambda res: res.value > 0
    )
]

with ptracer.context(callback, filter=flt):
    open('/tmp/null', 'wb')" > PtracerFiltering.py
```{{exec}}
