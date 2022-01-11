Reproduction of the pipenv bug with transitive local dependencies.

Clone this repo and run `pipenv lock` and it will fail with:

```
❯ pipenv lock --verbose
Locking [dev-packages] dependencies...
Locking [packages] dependencies...
Building requirements...
⠇ Locking.../Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/patched/notpip/_internal/operations/prepare.py:218: PipDeprecationWarning: DEPRECATION: A future pip version will change local packages to be built in-place without first copying to a temporary directory. We recommend you use --use-feature=in-tree-build to test your packages with this new behavior before it becomes the default.
 pip 21.3 will remove support for this functionality. You can find discussion regarding this at https://github.com/pypa/pip/issues/7555.
  deprecated(
Resolving dependencies...
Reporter.starting()
INFO:pipenv.patched.notpip._internal.resolution.resolvelib.reporter:Reporter.starting()
Reporter.adding_requirement(SpecifierRequirement('test-package-pipenv-base'), None)
INFO:pipenv.patched.notpip._internal.resolution.resolvelib.reporter:Reporter.adding_requirement(SpecifierRequirement('test-package-pipenv-base'), None)
CRITICAL:pipenv.patched.notpip._internal.resolution.resolvelib.factory:Could not find a version that satisfies the requirement test-package-pipenv-base (from versions: none)
Traceback (most recent call last):
  File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/patched/notpip/_vendor/resolvelib/resolvers.py", line 341, in resolve
    self._add_to_criteria(self.state.criteria, r, parent=None)
  File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/patched/notpip/_vendor/resolvelib/resolvers.py", line 173, in _add_to_criteria
    raise RequirementsConflicted(criterion)
pipenv.patched.notpip._vendor.resolvelib.resolvers.RequirementsConflicted: Requirements conflict: SpecifierRequirement('test-package-pipenv-base')
During handling of the above exception, another exception occurred:
Traceback (most recent call last):
  File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/patched/notpip/_internal/resolution/resolvelib/resolver.py", line 94, in resolve
    result = self._result = resolver.resolve(
  File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/patched/notpip/_vendor/resolvelib/resolvers.py", line 472, in resolve
    state = resolution.resolve(requirements, max_rounds=max_rounds)
  File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/patched/notpip/_vendor/resolvelib/resolvers.py", line 343, in resolve
    raise ResolutionImpossible(e.criterion.information)
pipenv.patched.notpip._vendor.resolvelib.resolvers.ResolutionImpossible: [RequirementInformation(requirement=SpecifierRequirement('test-package-pipenv-base'), parent=None)]
The above exception was the direct cause of the following exception:
Traceback (most recent call last):
  File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/utils.py", line 882, in resolve
    results = resolver.resolve(self.constraints, check_supported_wheels=False)
  File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/patched/notpip/_internal/resolution/resolvelib/resolver.py", line 103, in resolve
    raise error from e
pipenv.patched.notpip._internal.exceptions.DistributionNotFound: No matching distribution found for test-package-pipenv-base
During handling of the above exception, another exception occurred:
Traceback (most recent call last):
  File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/resolver.py", line 766, in <module>
    main()
  File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/resolver.py", line 760, in main
    _main(parsed.pre, parsed.clear, parsed.verbose, parsed.system, parsed.write,
  File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/resolver.py", line 743, in _main
    resolve_packages(pre, clear, verbose, system, write, requirements_dir, packages, dev)
  File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/resolver.py", line 704, in resolve_packages
    results, resolver = resolve(
  File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/resolver.py", line 685, in resolve
    return resolve_deps(
  File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/utils.py", line 1377, in resolve_deps
    results, hashes, markers_lookup, resolver, skipped = actually_resolve_deps(
  File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/utils.py", line 1106, in actually_resolve_deps
    resolver.resolve()
  File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/utils.py", line 884, in resolve
    raise ResolutionFailure(message=str(e))
pipenv.exceptions.ResolutionFailure: ERROR: No matching distribution found for test-package-pipenv-base
✘ Locking Failed! 
```

Whereas running `pip install -r requirements.txt` works:

```
❯ pip install -r requirements.txt
Processing ./test_package_pipenv_base
  Preparing metadata (setup.py) ... done
Processing ./test_package_pipenv_with_dependency
  Preparing metadata (setup.py) ... done
Building wheels for collected packages: test-package-pipenv-base, test-package-pipenv-with-dependency
  Building wheel for test-package-pipenv-base (setup.py) ... done
  Created wheel for test-package-pipenv-base: filename=test_package_pipenv_base-0.0.0-py3-none-any.whl size=1137 sha256=33abd1cc81df384b8f33b0494ea416a3e3eb6851959267da7720c72c4900d12f
  Stored in directory: /private/var/folders/6n/qx_2zxsd4sb5nblm9xzh76dw0000gn/T/pip-ephem-wheel-cache-bj6y34vy/wheels/84/ad/70/b3c26dda04b36cc40cbbf92510028cdbc9378a8134c758556a
  Building wheel for test-package-pipenv-with-dependency (setup.py) ... done
  Created wheel for test-package-pipenv-with-dependency: filename=test_package_pipenv_with_dependency-0.0.0-py3-none-any.whl size=1260 sha256=200c723b85d8aee55bb8f60ac6e757334838b7232861f4861e0a5620ef7ed6d9
  Stored in directory: /private/var/folders/6n/qx_2zxsd4sb5nblm9xzh76dw0000gn/T/pip-ephem-wheel-cache-bj6y34vy/wheels/1e/7f/fd/e485fb04ac144b4c0cdf09290458935407e8853e4ac18010cd
Successfully built test-package-pipenv-base test-package-pipenv-with-dependency
Installing collected packages: test-package-pipenv-base, test-package-pipenv-with-dependency
Successfully installed test-package-pipenv-base-0.0.0 test-package-pipenv-with-dependency-0.0.0
```
