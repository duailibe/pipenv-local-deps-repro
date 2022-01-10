Reproduction of a local transitive dependency on pipenv.

Clone this repo and run `pipenv lock` and it will fail with:

```
❯ pipenv lock
Locking [dev-packages] dependencies...
Locking [packages] dependencies...
Building requirements...
/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/patched/notpip/_internal/operations/prepare.py:218: PipDeprecationWarning: DEPRECATION: A future pip version will change local packages to be built in-place without first copying to a temporary directory. We recommend you use --use-feature=in-tree-build to test your packages with this new behavior before it becomes the default.
 pip 21.3 will remove support for this functionality. You can find discussion regarding this at https://github.com/pypa/pip/issues/7555.
  deprecated(
⠇ Locking.../Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/patched/notpip/_internal/operations/prepare.py:218: PipDeprecationWarning: DEPRECATION: A future pip version will change local packages to be built in-place without first copying to a temporary directory. We recommend you use --use-feature=in-tree-build to test your packages with this new behavior before it becomes the default.
 pip 21.3 will remove support for this functionality. You can find discussion regarding this at https://github.com/pypa/pip/issues/7555.
  deprecated(
Resolving dependencies...
✘ Locking Failed! 

CRITICAL:pipenv.patched.notpip._internal.resolution.resolvelib.factory:Could not find a version that satisfies the requirement test-package-pipenv-base (from versions: none)
[ResolutionFailure]:   File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/resolver.py", line 743, in _main
[ResolutionFailure]:       resolve_packages(pre, clear, verbose, system, write, requirements_dir, packages, dev)
[ResolutionFailure]:   File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/resolver.py", line 704, in resolve_packages
[ResolutionFailure]:       results, resolver = resolve(
[ResolutionFailure]:   File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/resolver.py", line 685, in resolve
[ResolutionFailure]:       return resolve_deps(
[ResolutionFailure]:   File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/utils.py", line 1377, in resolve_deps
[ResolutionFailure]:       results, hashes, markers_lookup, resolver, skipped = actually_resolve_deps(
[ResolutionFailure]:   File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/utils.py", line 1106, in actually_resolve_deps
[ResolutionFailure]:       resolver.resolve()
[ResolutionFailure]:   File "/Users/lucas/.local/pipx/venvs/pipenv/lib/python3.10/site-packages/pipenv/utils.py", line 884, in resolve
[ResolutionFailure]:       raise ResolutionFailure(message=str(e))
[pipenv.exceptions.ResolutionFailure]: Warning: Your dependencies could not be resolved. You likely have a mismatch in your sub-dependencies.
  You can use $ pipenv install --skip-lock to bypass this mechanism, then run $ pipenv graph to inspect the situation.
  Hint: try $ pipenv lock --pre if it is a pre-release dependency.
ERROR: No matching distribution found for test-package-pipenv-base

```
