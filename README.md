**pykit3** is a collection of python3 modules that are loved by [s2][] developers.

| Name | Desc |
| :-- | :-- |
| [k3color][] | create colored text on terminal |
| [k3common][] | dependency manager |
| [k3confloader][] | k3confloader loads conf for other pykit3 modules |
| [k3dict][] | It provides with several dict operation functions. |
| [k3down2][] | convert markdown segment into easy to transfer media sucha images. |
| [k3fmt][] | It provides with several string operation functions. |
| [k3fs][] | File-system Utilities |
| [k3git][] | wrapper of git command-line |
| [k3handy][] | handy alias of mostly used functions |
| [k3heap][] | k3heap is a binary min heap implemented with reference |
| [k3jobq][] | k3jobq processes a series of inputs with functions concurrently |
| [k3log][] | k3log is a collection of log utilities. |
| [k3math][] | k3math is a toy math impl |
| [k3net][] | Utility functions for network related operation. |
| [k3num][] | Convert number to human readable format in a string. |
| [k3pattern][] | Find common prefix of several `string`s, tuples of string, or other nested structure, recursively by default. |
| [k3portlock][] | k3protlock is a cross-process lock that is implemented with `tcp` port binding. |
| [k3priorityqueue][] | priorityQueue is a queue with priority support |
| [k3proc][] | easy to use `Popen` |
| [k3rangeset][] | segmented range which is represented in a list of sorted interleaving range. |
| [k3shell][] | A python module to manage commands. |
| [k3str][] | k3str is a collection of string utilities. |
| [k3thread][] | utility to create thread. |
| [k3time][] | Time convertion utils |
| [k3txutil][] | A collection of helper functions to implement transactional operations. |
| [k3ut][] | unittest util |


`pk3` is the super repo for pykit3 sub module repos
This is just a container and does nothing.

# Usage

```
pip install xxx
```


# Development

## Engineering practices

- Start with the correctness requirements, ignore the performance impact until
  the end. You'll usually write something faster by focusing on keeping things
  minimal anyway.

- Throw away what can't be done in a day of coding. when you rewrite it
  tomorrow, it will be simpler.

- Write code for human.

    - Clarity comes first.
    - Then the correctness.
    - Then the performance(It is python, right?).

    You are a smart guy. But your colleagues may not
    be smart as you are.

    **Do not write smart codes**.

- Only write comment to explain **WHY**.
  Improve the code to explain **HOW** and **WHAT**.

## Developing workflow

Follow the github flow:

- Fork a repo
- Hacking on it
- Fire a pull request and then merge it to upstream.

    - Rebase to upstream master before open a PR to reduce conflict.

- Open an issue to describe what you gonna do before hacking on it:
    Let everybody else know what you are doing.




## Make changes to a module

- `github-action` is used for CI, such as  https://github.com/pykit3/k3proc/actions .
    The CI action is defined in `.github/workflows/python-package.yml`.
    Most of the time you do not need to modify this file unless **you know what
    you are doing**.

- Testing is based on `pytest`: `pip install pytest` and `pytest`

- Document is generated by `sphinx`.
  Python docstring follows [google docstring style](https://www.sphinx-doc.org/en/1.5/ext/example_google.html).
  Documenting building stuffs are all in `docs/` dir.

  To build and test the doc: `make doc`.

- Github meta data such as description and project labels are managed with
  `.github/settings.yml`. see https://github.com/pykit3/gh-config for detail.

- A module repo needs a `__name__ = "k3proc"` and `__version__ = "0.1.2"` in its
  top level `__init__.py` so that building script generates the module name
  automatically. A `__version__` must be in [semantic-version][] syntax.

- Publish a module to `pip`:

    - Update the version in `__init__.py`, e.g., to publish next version
        `0.2.14` of `k3proc`, updata the `__version__` to `"0.2.14"`:

        ```
        # cat k3proc/__init__.py
        ...
        __version__ = "0.2.13"
        __name__ = 'k3proc'
        ...
        ```
    - Commit the version changes and `make release`.
        This step creates a new tag of the current version.

    - Pushing the created tag triggers an github action that
        build a pip module and upload to `pypi.org`.

    Then one can install the module(e.g. `k3proc`)
    with `pip install k3proc` or with version specified:
    `pip install k3proc==0.2.14`.


## Module dir layout

Take `k3proc` as example:

```
# github.com/pykit3/k3proc/

.github/workflows/python-package.yml
.github/workflows/python-publish.yml
.github/dependabot.yml
.github/settings.yml
_building/
docs/
test/
__init__.py
LICENSE
Makefile
proc.py*
README.md
requirements.txt
setup.py
```

- `.github/workflows/python-package.yml`: github-action for CI.

- `.github/workflows/python-publish.yml`: github-action for publishing a pip package.

- `.github/dependabot.yml`: dependency check config.

- `.github/settings.yml`: defines project desc, labels etc. Just modify it and push.

- `_building/`: building utils for build pip package and docs etc.

- `docs/source/conf.py`: universal doc generating config. DO NOT MODIFY.

- `docs/source/index.rst`: defines what module/class/function to generate doc.
    UPDATE this file for every modification to the codes.

- `test/`: unit test files.

- `test/data`: data file for test, if required.

- `__init__.py`: defines meta info about a module. such as name, version and
    module doc. Export APIs to end user.

- `LICENSE`

- `proc.py`: your actual works are here. choose whatever a name as you wish.
    It should be directly imported by `__init__.py` to expose APIs to end user.

- `README.md`: readme in pykit3 is generated. DO NOT MODIFY IT. `make readme`
    re-generates README.md by extracting docs and examples from codes.
    See: `_building/build_readme.py`.

- `requirements.txt`: the dependency pip. Some packages that are depended by
    all modules are defined in `_building/requirements.txt`.

- `setup.py`: script to publish the module. You do not need to modify this file.
    It is auto generated with `make release`.


## Create a new module

- Fork from [tmpl][], which is a template module.
    Choose a good name starts with `k3`, e.g. `k3whatever`.

- Clone the repo to your laptop:
  `git clone git@github.com:pykit3/k3whatever.git`.

- Populate it: `python ./_building/populate.py`.
    This step generates a skeleton of a module:
    ```
    python ./_building/populate.py
    git status
    ...
        new file：   .github/settings.yml
        new file：   __init__.py
        new file：   docs/source/index.rst
        new file：   package.json
        new file：   packages.txt
        new file：   requirements.txt
        new file：   test-requirements.txt
        new file：   test/test_doctest.py
        new file：   test/test_k3whatever.py
    ```

    Examine the generated files and add them and make your first commit!

## Document

Write the `f**king` doc!

- Document the module, classes and methods.

## Testing

Write the `f**king` test!


[s2]: https://github.com/bsc-s2
[semantic-version]: https://semver.org/
[tmpl]: https://github.com/pykit3/tmpl

[k3color]: https://github.com/pykit3/k3color
[k3common]: https://github.com/pykit3/k3common
[k3confloader]: https://github.com/pykit3/k3confloader
[k3dict]: https://github.com/pykit3/k3dict
[k3down2]: https://github.com/pykit3/k3down2
[k3fmt]: https://github.com/pykit3/k3fmt
[k3fs]: https://github.com/pykit3/k3fs
[k3git]: https://github.com/pykit3/k3git
[k3handy]: https://github.com/pykit3/k3handy
[k3heap]: https://github.com/pykit3/k3heap
[k3jobq]: https://github.com/pykit3/k3jobq
[k3log]: https://github.com/pykit3/k3log
[k3math]: https://github.com/pykit3/k3math
[k3net]: https://github.com/pykit3/k3net
[k3num]: https://github.com/pykit3/k3num
[k3pattern]: https://github.com/pykit3/k3pattern
[k3portlock]: https://github.com/pykit3/k3portlock
[k3priorityqueue]: https://github.com/pykit3/k3priorityqueue
[k3proc]: https://github.com/pykit3/k3proc
[k3rangeset]: https://github.com/pykit3/k3rangeset
[k3shell]: https://github.com/pykit3/k3shell
[k3str]: https://github.com/pykit3/k3str
[k3thread]: https://github.com/pykit3/k3thread
[k3time]: https://github.com/pykit3/k3time
[k3txutil]: https://github.com/pykit3/k3txutil
[k3ut]: https://github.com/pykit3/k3ut