# python-preprocessor

This is sort of a preprocesser for python. It allows you to automatically make changes to code before it is run. I tried to keep this as small as possible.

Paste one of these at the top of your file, provide a `process` function, and it does the rest (in theory). The actual code you want to process goes below the preprocessor.

Main version:
```python
import sys
def process(lines):
  # lines is a list of all of the lines of code that are not part of the preprocesser.
  # the "process" function should return a new version of the original "lines" list.
  return lines

code = __import__("inspect").getsource(sys.modules[__name__])
lines = code.split('\n')
for line in lines:
  x = globals().get('x',0) + 1
  if line == 'sys.exit()':break
exec('\n'.join(process(lines[x:])))
sys.exit()
#######################################

print("Hello World!")
```

Snekbox-compatability version. Don't use unless you need to run in Snekbox. It also runs in regular cpython, but its longer.
```python
import sys
def process(lines):
  # lines is a list of all of the lines of code that are not part of the preprocesser.
  # the "process" function should return a new version of the original "lines" list.
  return lines

snekbox=("built-in"in str(sys.modules[__name__]))
if snekbox:
  #from chilaxan on https://discord.gg/python (message link: https://discord.com/channels/267624335836053506/470884583684964352/836774107609694218)
  from ctypes import POINTER,c_wchar_p,c_int,pythonapi,byref
  _argv = POINTER(c_wchar_p)()
  _argc = c_int()
  pythonapi.Py_GetArgcArgv(byref(c_int()), byref(POINTER(c_wchar_p)()))
  code = _argv[:_argc.value][-1]
else:code = __import__("inspect").getsource(sys.modules[__name__])
lines = code.split('\n')
for line in lines:
  x = globals().get('x',0) + 1
  if line == 'sys.exit()':break
exec('\n'.join(process(lines[x:])))
sys.exit()
#######################################

print("Hello World!")
```

