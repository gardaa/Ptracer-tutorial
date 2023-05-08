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
    open("/dev/null", "wb")' > PtracerTutorial.py
```{{exec}}

We can now run the script
```
python PtracerTutorial.py
```{{exec}}

As you can see, the script manages to trace down 5 different system calls just by opening an arbitrary file. These are the following system calls, and if you look at the terminal to the right, you can also see the arguments inside the parethesis and the result after the arrow:
- openat
- fstat
- ioctl
- lseek
- close

Below the line that says traceback, you can also see which line the system call occured. In this case all of them occured in line 11, as we only executed a single open function in this example. As mentioned in the introduction, getting the exact location of the relevant system call helps debugging more efficiently. 



    
