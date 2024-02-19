# Setting up a Python repository

> This document describes how to guide participants through setting up a new repository from scratch for the Python lanugage, and in this case we are using ATM as the kata.  At the end the participants should have all of the pytest tooling installed, a new repository locally, and it also remotely, with pushed to github.com.

When setting up a new repository from scratch, there are a number of steps to successfully complete it.  Rather than trying to copy it from another repository and correct it, here we take the approach of guiding you to set up a new repository from scratch with PyTest as the testing library.

We will go through setting up a new repository for the ATM kata, so that we can use it in VSCode.

1. Setup the local repository  
Open a terminal and navigate to the directory that you going to store your katas in e.g. `~/code/katas`

Run:

```sh
git init atm
```

This initializes the git repository locally and also creates the atm directory

To see it worked :

```sh
cd atm
git status 
```

You should see an output like this:

```sh
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

2. Install the packages you need  
Open the atm directory within VSCode, and then in VSCode open the terminal there (by going to the Terminal menu item at the top of the Window and choosing New Terminal).  In the terminal run:

```sh
pipenv install pytest pytest-cov pytest-watch pytest-pspec pytest-describe --dev
```

This creates a new Pip environment where each of the packages is installed.  We are using Pytest and extensions of Pytest to do our testing.

After you have run that command successfully you will see that you have a _Pipfile_, which lists all of the packages you've installed as development packages.  You also have a _Pipfile.lock_ which lists out what exactly is installed, along with the dependencies and the versions used.

3. Configure pytest  
Next, we have to configure _pytest_.  So you can create a pytest.ini file, and in it paste:

```python
[pytest]
addopts = --pspec --cov-config=.coveragerc --cov-report=term-missing --cov='.' --rootdir='.' --tb=long -l -vv 
```

This is how we configure how _pytest_ runs.  
`--pspec` - is used to format the output of what tests are running in an easy to understand format
`--cov-config` - is used to configure coverage, and we will use the .coveragerc file that we will create in the next step  
`--cov-report` - we specify the type of coverage report we want.  We use term-missing to include a column with the missing lines that are not covered  
`--cov` - specifies that we are using the current directory for coverage  
`--rootdir` - specifies that the current directory is our root directory  
`--tb` - configure the traceback, and we set it to long.  This means if there is an error we get the full output of the error message  
`-l` - enables the locals in the traceback  
`-vv` - specifies that the output is very verbose  

4. Configure code coverage  
So we talked about the `.coveragerc` file, and now we go ahead and create it. Once you have created the file, paste the following contents

```python
[run]
omit =
    # omit tests from the coverage
    tests/*
```

> The indentation is important, and this is telling the coverage reporting to ignore the tests folder, as we want to know the coverage of the implementation files but it doesn't make sense to have more tests testing the tests themselves.

5. Create our test  
You may have noticed that we talked about a tests folder, so let's create the file `tests/test_atm.py`, you can create a new file with that name, and it will end up creating the tests folder.

In  _tests/test_atm.py_ we are going to create a describe function, and inside of that, we will add a basic test.

```python
def describe_atm():
    def should_validate_true():
        """🧪 expect the dummy function to be true"""
        assert True == True
```

Save the file with the contents above.  Then in your terminal run
pipenv run pytest

This will give you output like:

```sh
...
plugins: metadata-3.1.0, pspec-0.0.4, html-4.1.1, cov-4.1.0, describe-2.1.0, mock-3.6.1, describe-it-0.1.0
collected 1 item                                                        

tests/test_atm.py::describe_atm::🧪 expect the validation to be true        
                                                                  
describe_atm
 ✓ 🧪 expect the validation to be true
....
=========================== 1 passed in 0.03s ===========================
```

So we can see that the test ran successfully

We also see an error about coverage which we expect:

```sh
...CoverageWarning: No data was collected. (no-data-collected)
  self._warn("No data was collected.", slug="no-data-collected")
WARNING: Failed to generate report: No data to report.
...
=========================== 1 passed in 0.03s ===========================
```

We haven't created any files for coverage yet, so we expect this to happen.

If you go back to the test and remove the line with:

```python
"""🧪 expect the validation to be true"""
```

and re-run pipenv run pytest, you will see that the output has changed to:

```sh
...
tests/test_atm.py::describe_atm::should_validate_true    
                                                                      
describe_atm
 ✓ should validate true
...
```

So the text in the triple double quotes allows you to give a more descriptive message as to what the test does.

6. Commit our changes  
You can commit all of your changes by running:

```sh
git add .
git commit -m "initial commit
```

We don't have a remote repository yet to push it to.  You can do that by going to [https://github.com](https://github.com) in your browser and creating a new repository called atm.

Once created you will have instructions on what to do next.  One set of instructions will have a message:

> …or push an existing repository from the command line  

You can copy the three commands and run them from your terminal in VSCode.

Now that you've pushed your code, refresh your page in your browser and you should see all of the files there.

7. Update our test to call our dummy function  
Now we can move on to address our coverage.  Let's update `tests/test_atm.py`, so that we pick up our `modules/atm.py` that we will create, and call the dummy function within it:

```python
from modules import atm


def describe_atm():
    def should_validate_true():
        """🧪 expect the dummy function to be true"""
        assert atm.dummy() == True
```

Note, the first and last lines of the file have changed.

Rather than having to run the test command all of the time, we are going to update the Pipfile so that we can run a watch command, which will execute the tests every time we save a python file.

At the end of the Pipfile add the following code:

```python
[scripts]
tests = "pytest"
watchTests = "ptw"
```

`ptw` is the command for `pytest-watch`.

Now in your terminal run:
pipenv run tests
So that we can see that the tests command works

You will see that the output errors with:

```sh
...
sDW/lib/python3.11/site-packages/pytest_watch/command.py", line 109, in main
    return watch(entries=directories,
           ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/bob/.local/share/virtualenvs/trial_python-hVuRKsDW/lib/python3.11/site-packages/pytest_watch/watcher.py", line 237, in watch
    raise ValueError('Directory not found: ' + entry)
ValueError: Directory not found: /Users/bob/code/katas/trial_python/modules
...
```

8. Add our implementation  
So let's go ahead and create the file `modules/atm.py`, in that file, you should create a function called `dummy()` which returns True.

Then run the pipenv run watchTests command, and you should see something like:

```sh
____________________ ERROR collecting tests/test_atm.py ____________________
ImportError while importing test module '/Users/bob/code/katas/trial_python/tests/test_atm.py'.
Hint: make sure your test modules/packages have valid Python names.
Traceback:
...
tests/test_atm.py:1: in <module>
    from modules import atm
E   ModuleNotFoundError: No module named 'modules'
...
```

9. Add our `__init__.py` files  
This shows that we're missing two important files `__init__.py` .  Go ahead and create:

```sh
tests/__init__.py
modules/__init__.py
```

You may have noticed that your tests are now passing with output like:

```sh
...
tests/test_atm.py::describe_atm::🧪 expect the validation to be true                                                                             
describe_atm
 ✓ 🧪 expect the validation to be true


---------- coverage: platform darwin, python 3.11.6-final-0 ----------
Name                  Stmts   Miss  Cover   Missing
---------------------------------------------------
modules/__init__.py       0      0   100%
modules/atm.py            2      0   100%
---------------------------------------------------
TOTAL                     2      0   100%


============================ 1 passed in 0.03s =============================
```

10. Push code to GitHub  
Now that we have completed the test, let's command and push that:

```sh
git add .
git commit -m "added the dummy function"
git push
```

Check your browser to see the `modules` folder with the `atm.py` file.
