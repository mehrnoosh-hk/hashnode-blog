## Python concurrency Part 1: 
Understanding Python GIL

**Global Interpreter Lock** (GIL) is a controversial topic in Python. Essentially, it means that GIL prevents any Python process from running more than one Python bytecode at any given time. 

So this means that even if the system has CPUs with more than one core, just one Python instruction can be executed at any moment. 

So why does GIL exist?

The answer is how CPython (the standard implementation of Python) manages memory and garbage collection. In a nutshell, CPython keeps track of objects in memory by reference counting. This means that whenever an object is created in the memory, CPython keeps track of all the instances that referenced that particular object. When the reference count reaches zero, no one references the object, and it can be deleted from memory. The problem is that this implementation is not thread-safe. Not being thread-safe means that unexpected results may happen if more than one thread tries to modify a shared variable. This behaviour is also known as Race Condition.