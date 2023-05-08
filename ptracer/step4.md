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

This filter will ensure that only **'openat'** calls will be displayed and it can be very useful for very verbose executions.\
To set up a filter we use SysCallPattern, this object matches names, arguments and results of system calls and enables filtering.\
Each pattern value can be:
* A callable that receives a `SysCallArg` or a `SysCallResult` instance and returns a boolean.
* An object with a `match()` method that received an unpacked value of a syscall attribute and returns a boolean.
* A regular expression object. For example: `SysCallPattern(name=re.compile('open.*')).Any`.

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
As before we can execute it with
```
python PtracerFiltering.py
```

We notice how, compared to the previous output, the result is much shorter and includes only *'openat'* calls.
