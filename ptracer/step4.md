# Filter

Ptracer allows elaborate syscall filtering via the filter argument, we can upgrade the previous program like this:
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

