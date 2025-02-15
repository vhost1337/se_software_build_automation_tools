## Exercise 2

### Preparation

1. Follow the project's README.md [hello_world](./hello_world) to run locally and test the tests.

2. Add `Makefile` to wrap the dependency installation, run, and test commands with Makefile targets:

   ```makefile
   .PHONY: deps test

   deps:
   	pip install -r requirements.txt; \
   		pip install -r test_requirements.txt
   ```

   Notice: we use tabs in `Makefile`.

3. Test each of the `Makefile` targets.

4. Create a git repository for the hello_world application in your github account, e.g., python_simplec_ci.

5. Please do not foget about adding a `.gitignore`, you could start with:

   ```
   # Byte-compiled / optimized
   __pycache__/
   *.py[cod]

   # venv
   .venv
   venv/
   ```

6. Push all the code to the github repository.

7. You github repository should look like that:

   ```
   |- hello_world/
   |- test/
   |- main.py
   |- Makefile
   |- README.md
   |- requirements.txt
   \- test_requirements.txt
   ```

### Continuous Integration with Github Actions

We will show how to build a Continuous Integration pipeline with [Github Actions](https://docs.github.com/en/actions). Basic concepts after [docs](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions)

- Event - trigger workflows
- Workflow - `.github/workflows`
- Job
- Steps
- Action - reusable complex task

1. Create in the root of your project a directory -  `.github/workflows`:

   ```bash
   mkdir -p .github/workflows
   ```

2. Let's create the first automation - `.github/workflows/ci.yaml` (after [github docs](https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-nodejs-or-python?langId=py)):

   ```yaml
   name: Package Project

   # Controls when the workflow will run
   on: [ push ]

   jobs:
     build:
       runs-on: ubuntu-latest
   
       steps:
         # get the code under $GITHUB_WORKSPACE directory
         - uses: actions/checkout@v2
         # get the python
         - name: Set up Python 3
           uses: actions/setup-python@v3
         - name: Install deps
           run: pip3 install -r requirements.txt
   ```

3. Push the workflow file `ci.yaml` to your github. Please open *Actions* in your repository web view and see whether the automation works.

4. Now, let's add more tooling. Please call lint and run tests with Github Actions.

5. Replace calling directly `pip3`, linter and executing tests by calling `Makefile` targets.

## More tooling

1. Test coverage:

   ```bash
   echo 'pytest-cov' >> test_requirements.txt
   pip install -r test_requirements.txt
   ```

   ```bash
   PYTHONPATH=. py.test --verbose -s --cov=hello_world --cov-report=xml
   ```

2. [black](https://github.com/psf/black) uncompromising code formatter:

   ```bash
   pip install black
   black hello_world

   # to see the changes
   git diff
   ``` 

   Notice, if you are still learning Python, do not use *black*. With *flake8*, you must to fix the issues yourself and learn. You will miss this learning oportunity with *black* and learn slower.

## Additional materials

- https://github.com/mre/awesome-static-analysis#python 
- Commit messages: 
  - Imperative: https://chris.beams.io/posts/git-commit/
  - Semantic:
    - https://seesparkbox.com/foundry/semantic_commit_messages
    - https://www.conventionalcommits.org
  - Gaining some popularity: https://github.com/carloscuesta/gitmoji-cli
