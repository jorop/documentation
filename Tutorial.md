# Tutorial
Welcome to the Cloudomation tutorial. The tutorial has several chapters. Feel free to jump directly to a chapter that interests you, or start at the top and read through the entire tutorial.  

The tutorial is intended to give you a first idea about the Cloudomation platform, its functionality, some concepts that we use in the documentation, and to get you started with your own explorations of Cloudomation. Not all Cloudomation functionality is covered in this tutorial.  

## Some vocabulary
In Cloudomation, you define automations using flow scripts, or **flows**. Flow scripts are written in Python. You can think of them as the units or pieces of your automation.  

Running a flow script creates an **execution**. An execution contains all the information about a specific run of a flow script - inputs, outputs, runtime, etc.  

An execution can start several **child executions**. A child execution is any execution that is started by another execution, which usually provides the child execution with inputs, with child executions usually also returning something back to the **parent execution** - for example an output, or a status.

So: you write a flow script. When you run it, it creates an execution. Your execution can be the parent execution of child executions.  

There are different types of functionality that Cloudomation provides. They are structured in three categories:
- What happens *within* a flow script
- What happens *on the Cloudomation platform*
- What happens *outside of the Cloudomation platform*

The first category, things that happen within a flow script, can be things like setting the outputs of your flow script, or setting the flow script ended status. Within a flow script, you would use the **this** handler for these things. As an example, `this.success(message='all done')` sets the success message at the end of the flow script to "all done". *This* is similar to *self* in Python. We named it *this* to avoid confusion with *self*, but if you are used to using *self* you will get a handle on *this* quickly.

The second category, things that happen on the Cloudomation platform, can be things like reading or writing a file or a system setting. Within a flow script, you would use the **system** handler for these things - for anything that happens on the Cloudomation system, but not just inside of your flow script. As an example, `system.setting('mysetting')` refers to a setting called "mysetting".

The third category is what we call **tasks**. Tasks allow you to interact with the outside world. For example, you can use a task to connect to a server via ssh, or to send an email, or to call a REST API. Tasks are pre-defined executions that have specific inputs and outputs, which are defined by us. So when you execute a task, you always create a child execution for this task.

## How to work with the documentation

Everything that you can use with *this* and *system* is documented in the [Cloudomation flow script method reference](Flow+script+function+reference). Tasks are documented separately: [Tasks](Tasks). I will describe what you can find in both of those pages.

Documentation is always alive, and ours will change and grow together with the Cloudomation platform. If you find pages in the documentation that are out of date or are missing something, please let us know. Drop us a line to [info@cloudomation.com](mailto:info@cloudomation.com).

### An introduction to the Cloudomation flow script method reference
The [Cloudomation flow script method reference](Flow+script+function+reference) contains documentation about all the functionality that you can use in your flow scripts. However it is intended as a reference, so it only lists arguments and fields and is generally very brief. It is automatically generated, so you can be sure that the flow script method reference is always up to date.

The [Cloudomation flow script method reference](Flow+script+function+reference) starts with a list of classes, and a list of enums. Enums is short for enumerations. I will explain in a bit what they are.

#### Classes

There are several classes available to you on Cloudomation. There is absolutely no need for you to fully understand what a Python class is in order to be able to use it. Here is what you need to know:

Classes define the properties of objects. Python is an object-oriented language. This means is that you work a lot with objects.

The word object is already a very good description of what an object is: it is a thing that has some properties. Now in Python, every object has a class. And the class is what defines which properties an object has.

What is a property, you ask? A name is a property, a length, a color, a description, anything that says something about an object. In Python, every object can only have a certain set of properties. And which properties an object can have is defined in the class.

The Cloudomation classes have three sets of properties: they have arguments, constants, and methods.

Arguments are things that you can specify when calling a method.

Constants are values that are associated with an object.

Methods are operations you can do with an object.

Let's look at an example. Let us take the class setting. I can call an instance of the class setting like so: `system.setting('mysetting')`. Now I have referenced "mysetting", which is an object, and it is of the class setting.

If you look at the [Cloudomation flow script method reference](Flow+script+function+reference) and read up on setting, you will find a list of arguments, constants, and methods for the class setting.

There is one more thing that is important to know about classes: classes can have base classes from which they inherit properties. If you look at the setting class in the Cloudomation method reference, you will see that it says "base class: record". This tells you that the setting class, together with some other classes like execution, file, flow etc. are all sub-classes of the records class. This is not hugely important to understand the documentation - all the inherited properties are listed again for each of the sub-classes. But later, it might be interesting to know which classes share properties. For example, there are some methods that you can apply on any record: you can save and load and delete and check if it exists, for example. It works exactly the same for every instance of a record class, which means it works exactly the same for all instances of sub-classes of the record class. So you save an execution and a file and a setting in exactly the same way - once you know how to do it for one, you can do it for all.

##### arguments

The argument names are not always straightforward. Let's start at the top.

Every class has an argument *select*. The select argument allows you to specify which object you want to access. Each class has a different identifying argument. If we stick with our setting example, settings are identified by their name. So if I want to reference a setting, I would need to use its name for the select argument.

If spelled out explicitly, our previous example would look like this:  
`system.setting(select='mysetting')`.

Let us take a closer look at all the arguments for the setting class. The principle is the same for all other classes as well.

Select is the argument you use to say which setting you want. It has to be the identifying argument for that object. Each class (and therefore each object that is of that class) has a specific identifying argument, which is usually either the name or the ID. For setting, the identifying argument is the name. In the documentation, the parenthesis next to the argument tell you in which format this argument has to be given. The select argument has to be a str, which is short for string. You specify a string by using '' quotes.

Init is for all the fields that you can set for an object when you call it. Those are the fields listed under constants: the create fields and the update fields. Which fields these are depends on whether the object already exists (update fields) or if you are creating it (create fields). We will take a look at the constants a bit further down. The init argument has to be given in the format of dict, which is short for dictionary. Dictionaries consist of key: value pairs in {} curly brackets (and can contain several objects and nested objects etc., let's stick with the simple case of key-value pairs for now).

Save says whether or not you want to save your setting. The default value is True, so by default your setting will be saved unless you specify that it shouldn't be saved. The save argument has to be given as bool, i.e. boolean value, which means True or False.

Kwargs is short for keyword arguments. Keyword arguments are basically anything that you want, you can add any key-value pairs here. It says in the documentation what will happen with additional keyword arguments that you specify. In our example of the settings class, it says that all keyword arguments will be added to the init argument. This makes things easier for you, but can also be a bit confusing when you first start out. Before we understand what the init argument is and which fields it contains, we can't really use the kwargs sensibly, so we will also look at that a bit later.

Now that we know what arguments are, you still need to know how to use them: you specify them in parenthesis after you call the setting class, and you need to separate each argument from the next with a comma:

`system.setting(select='mysetting', init={'value': 'myvalue'}, save=True)`

Here, we call a setting with the name "mysetting" and specified that the value should be "myvalue". We also said that it should be saved.

Python tries to make things easy for you, so you don't necessarily have to name each argument - `system.setting('mysetting')` works just as fine and does the same. If you do not name them, it is important that you stick to the right order which you can look up in the method reference. For setting, the order is: select, init, save, \*\*kwargs.  
You can also mix named and unnamed arguments. If you do this, you have to start with the unnamed arguments and specify them in the right order, and specify named arguments at the end (in any order). In our example, the order of arguments doesn't matter because we named them all. Let's look at some examples:

`system.setting(init={'value': 'myvalue'}, save=True, select='mysetting')` - I changed the order, but since I named all arguments it doesn't matter.

`system.setting('mysetting', {'value': 'myvalue'}, True)` - I am not naming any of the arguments, but I used the right order, so they will be recognised.

`system.setting('mysetting', save=True)` - I left out the init argument, so I needed to name the save argument because I put it in second place - if I hadn't named it, it would have taken "True" as the init argument.

**optional arguments and default values**

One more thing: you don't have to specify all arguments. Actually, in most cases, you do not need to specify any argument, they are all optional. If you write `system.setting()` you just reference an empty instance of a setting object. You can do that, and you can use it later, if you want.  

If you don't specify a argument, its default value will be used. You find the default values for all arguments in the very first line of the documentation for that class, in parenthesis after the class name. For the settings class, the defaults are:

select=None  
init=None  
save=True  

There are no defaults for the keyword arguments, they are fully optional - there is no need to have any keyword arguments at all.

This means that if you don't specify any arguments, your setting will have no name, no values for the init fields (we're getting to that in the next chapter), and will be saved by default. So for the above example, we wouldn't have needed to say `save=True`, it would have been like that by default anyway. You can specify it nevertheless for readability, if you like, it doesn't make a difference for the computer, just for you who is reading the flow script.

##### Constants

Constants are values. They are called constants because you cannot change them - that is, you cannot change which constants there are and how they are named. But you can change their content.

There are three types of constants that are important for you: the fields. You have fields, create\_fields and update\_fields. These fields contain information. Some of them are filled by the system, some of them can be filled by you.

Create fields are ones that you can specify when you first create an object. If we stick to our example of a setting, I can create a setting by specifying its name and value.

The update fields are fields I can change for an already existing object. Again for the setting example, I can update the name and value.

The fields are all the information that is associated with the object, and you can ask for all these fields for an existing object. If we take a setting as an example, we can get information about its creation date and time by requesting the field "created\_at". All the fields that are not in the create and update fields list are automatically generated by the system and cannot be modified by you (not directly, you can of course modify a modified\_at timestamp indirectly by modifying the object).

The create and update fields are what can be given as the init argument for an object. The init argument has to be a dictionary which can contain at most the values of the create fields (if you create the object) or of the update fields (if the object already exists and you want to update it). It is important to note that creating and updating can look exactly the same in the flow script, and you can overwrite object values without realising if you don't first check if the object already exists.

Let's take the above example of our setting:

`system.setting(select='mysetting', init={'value': 'myvalue'})`

This will either create a new setting with the name "mysetting" and the value "myvalue" if it doesn't already exist. If a setting with the name "mysetting" does already exist, it will update the value, i.e. overwrite the existing value with "myvalue". We leave out the save argument because we like the default.

A setting can contain much more than just one simple string as its value, so the init dictionary could also look more complex. For example, we could create a setting that contains a list:

`system.setting(select='fruit_salad', init={'value': ['apples', 'bananas', 'oranges']})`

Or we can create a setting that contains several key-value pairs:

`system.setting(select='colors', init={'value': {'sun': 'yellow', 'ocean': 'blue', 'grass': 'green'}})`

Now you can see that this is getting a bit messy: specifying a dictionary by hand can be a bit of a drag. This is where the kwargs come in handy. In the documentation for the settings class, it says that all keyword arguments will be added to the init argument. So what you can do is just add things at the end of your arguments which will be added to the value of your setting:

```python
system.setting(select = 'mysetting', value = 'myvalue', save = True)
system.setting(select = 'fruit_salad', value = ['apples', 'bananas', 'oranges'], save = True)
system.setting(select = 'colors', value = {'sun': 'yellow', 'ocean': 'blue', 'grass': 'green'}, save = True)
```
Each of the above lines you can use to both create and update an object.

##### methods
By now we know how to use the arguments and constants of a class. Now we will take a look at methods that we can apply on objects of a class.

You can think of methods as actions, things that you do with an object. In the [Flow script method reference](Flow+script+function+reference) you will see that all methods have an expand arrow that allows you to see the details of the method. Like classes, methods have arguments which allow you to say how something should be done. And like for classes, you can see the method's defaults in the parenthesis right next to the method name.

Once you expand a method, you will see a short description of what it does, its arguments, and what it returns. Often, a method will return the object on which the method was performed, but sometimes it will return something else.

Let's take a look at some of the methods for the settings class. You can apply the methods just like you would apply any other Python method, by just adding a .method() call to the reference of your object.

Let's start with the exists() method. It lets you check whether or not an object already exists. This comes in handy when you want to avoid overwriting existing objects.

`system.setting(select='mysetting').exists()`

This will return True if there already is a setting with the name "mysetting", and return False if there isn't. You can use this for example like so:

```python
if not system.setting(select='mysetting').exists():
    system.setting(select='mysetting', value='myvalue')
```

This will check if the setting exists and create it if it doesn't exist.

Now let's take a look at the save() method. We know that the setting class (and all record classes) have a save argument, which does the same as the save method, so you can use them interchangeably. The handy thing here is that you can apply the save() method also to many other objects, so you can keep your script consistent and readable by always using the save() method instead of the save argument. However in terms of performance, it is better (faster) to use the argument, because arguments are included in one system call, while applied methods are done in separate system calls. The following two lines are methodally equivalent, but the first requires only one system call and will be faster, while the second one requires two system calls:

`system.setting(select='mysetting', value='myvalue', save=True)` - one system call

`system.setting(select='mysetting', value='myvalue').save()` - two system calls

For saving a small setting you will not know the difference, but when the setting becomes very large, or you want to apply many different methods on objects, it can become handy to be aware of these finer points.  
Note that this is not an ideal example - the default value for save is already True for the setting class, so adding .save() will actually not make a difference (besides triggering a second system call) because the setting will already have been saved with the first call.

We should also take a look at the difference between set() and save(), as well as get() and load(). Set() and get() operate in the cache. The cache is a temporary store that exists only for the duration of the runtime of your script. This means that you can put things in the cache, get them from the cache, and work with them, but once the script has run its course, the cache will be emptied and whatever was in there will be gone.

Save() and load() on the other hand interact with permanent storage. Anything you save() will be there after your script has ended, and anything you load() will be loaded from permanent storage.

This means that you can have different values for the same object in the cache and in the permanent storage - if you set things in the cache, but don't save them.

Let us take a look at how this works:

```python
system.setting(select='mysetting', value='myvalue', save=True) # I create a setting with the name "mysetting" and the value "myvalue". It is saved by default but to be extra clear, I specified the save paramenter explicitly. This means that this setting now exists in permanent storage with the value "myvalue".

mysetting = system.setting(select='mysetting').set(value='another value') # Now I set the value to "another value". I do not save after setting the value. So now I have "myvalue" still in permanent storage, but I have "another value" in the cache.

getsetting = mysetting.get('value') # Get() reads the cache. So this will return "another value".

loadsetting = mysetting.load('value') # Load() reads from permanent storage. So this will return "myvalue". Note that load also updates the cache, so now we have "myvalue" in the cache.

getsetting2 = mysetting.get('value') # Since we loaded "myvalue" into the cache, this will not return "myvalue".

this.log(getsetting, loadsetting, getsetting2) # If you run this snippet and look at the output, you will see which different values are returned.
```

There are a few other methods available for the setting class, such as delete(), clone() and update(). Now that you know how to work with the documentation, why don't you go ahead and try if you can use them?

#### Enumerations
Enumerations, or enums, are lists of possible values that the system can return for certain events. Let us take a look at one enumeration as an example: the return\_when enumeration.

Return\_when lists all possible values that the system can return for running executions. I can use this when I want to start several executions in parallel, and want to wait for either all of them or one of them to end, or if I want to wait until all of them have ended successfully. These are the three conditions currently available in the return\_when enumeration:
* all\_ended - means that all executions have ended. This does not necessarily mean that they ended successfully, it can also mean that they ended with an error.
* all\_succeeded - means that all of them ended successfully.
* first\_ended - means that the first execution of a list of executions has ended, again could be with an error or successfully.

The documentation tells you when you can use these enumerations. The other enumeration, status, lists all possible statuses in which an execution can be. You can use this to trigger certain events, e.g. deleting old execution records when there is an execution that is waiting for token.

### An introduction to the Tasks documentation
Tasks are documented separately. One thing that is important to understand is that tasks are also executions. This means that you can interact with them like with any other execution. Starting a task creates an execution object that you can interact with like you can interact with any other execution object.

So what are tasks? We have mentioned that functionality to interact with "the outside world", i.e. anything outside the Cloudomation platform, happens in the form of tasks.

The following tasks are currently available on the Cloudomation platform:
- REST - communicate with REST APIs
- SSH - connect to remote systems with ssh (e.g. to execute a script there)
- GIT - check out a git repository (e.g. to get flow scripts and settings from git)
- SMTP - send emails via SMTP
- INPUT - request input from a user in the Cloudomation platform
- AWS - communicate with AWS
- REDIS - access a REDIS database

The task documentation lists the inputs and outputs that are available for each task, and provides you with examples on how to use each of the tasks. Take a look: [Tasks](Tasks).

## Next steps
There are a few more documentation pages that might help you get started with your first project.

Take a look at the [quick start guide](Quick+start) for a first impression. Review the  [Examples](Examples) to get an idea of how more complex flow scripts could look like.    

The [Flows](Flows) site provides you with information about writing, editing and deleting flow scripts and helps you find your way around the flow script editor and user interface.

We also recommend to review the [Tips and tricks for writing flow scripts](Tips+and+tricks+for+writing+flow+scripts) before you get started with your own project.  

It might also be a good idea to take a look at the [public flow script library](https://github.com/starflows/library){ext} to see if there are some snippets or entire flow scripts that you could reuse.
