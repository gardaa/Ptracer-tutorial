# Capture your first data

We can now create a Python file where we will write our first script
```
touch PtracerTutorial.py
```{{exec}}

This is our first python script, we use ptracer and traceback to trace calls made by the **'open'** function and print the arguments and result of each call. **traceback** module provides a way to print the call stack for debugging purposes. \


```
import traceback
import ptracer

def callback(syscall):
    print('{}({}) -> {}'.format(
        syscall.name,
        ', '.join(repr(arg.value) for arg in syscall.args),
        syscall.result))
    print('Traceback: ')
    print(''.join(traceback.format_list(syscall.traceback)))

with ptracer.context(callback):
    open('/dev/null', 'wb')
```

The context method of the ptracer module is used as a context manager to enable tracing of system calls made within the block of code.\
The callback function is called each time a system call is made, and it prints the name of the call, its arguments, and its result.\
The open function is called once to open the /dev/null file in binary write mode.\
We now write ths code in the file we created in the first step
```
echo 'import traceback
import ptracer
def callback(syscall):
    print("{}({}) -> {}".format(
        syscall.name,
        ", ".join(repr(arg.value) for arg in syscall.args),
        syscall.result))
    print("Traceback: ")
    print("".join(traceback.format_list(syscall.traceback)))
with ptracer.context(callback):
    open("/dev/null", "wb")' > ptracer_example.py
```{{exec}}

We can now run the script
```
python PtracerTutorial.py
```{{exec}}



    
