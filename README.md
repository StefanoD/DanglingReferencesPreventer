# Dangling References Preventer

Despite smart pointers since C++ 11, it is still easy to create a dangling reference in C++:

```cpp
std::future<int> foo(int a)
{
    return std::async(std::launch::async, [&](){
        return a + 1;
    });
}
```
In this example, where the body of the future is a lambda expression, the body gets executed after the stack of the method `foo()` has been cleaned up. That is, the reference to the variable `a` is not valid anymore.

This projects tries to prevent at least the dangling pointers which can occure by implicit reference captures in lamdas which many developer write, because of convenience. 

This check gets installed as a git hook, which is nothing else than a git plugin. In this case the check occures before the code gets successfully comitted:

```
~$ git add my_code.cpp
~$ git commit -am "test"
Error in file my_code.cpp:52: implicit reference capture is used in lambda expression:
return std::async(std::launch::async, [&](){
```

It is therefore better to *explicitly* write the intention of the developer:

```cpp
std::future<int> foo(int a)
{
    return std::async(std::launch::async, [a](){
        return a + 1;
    });
}
```

## Installing the git hook

The script can be installed by copying the script `pre-commit` to the folder `.git/hooks`. 

Because this file gets *not* committed to the repository, it is recommended in a project with more than one developer, to commit the script in a central "workspace setup folder" and install the git hook by an automated process, like a copy command in a CMake file.

You'll find an example in the root folder of this project.
