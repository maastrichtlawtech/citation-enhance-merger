# Changelog

All notable changes to this project will be documented in this file.

## 20-08-2020

- [`f1f3aa0`] Add workflow to update the requirements for python 3.7

Created action on every push to run [pigar](https://github.com/damnever/pigar) to generating `requirements.txt` from python files and notebooks.

```yaml
# Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
- uses: actions/checkout@v2

- name: Set up Python 3.7
  uses: actions/setup-python@v2
  with:
    python-version: 3.7

- name: Install pigar
  run: |
    python -m pip install --upgrade pip
    pip install pigar

# Use pigar. See options https://github.com/damnever/pigar 
- name: Generate requirements with pigar
  run: |
    pigar
   
- uses: stefanzweifel/git-auto-commit-action@v4
  with:
    commit_message: 🤖 Generate requirements for python 3.7
    commit_user_name: Requirements Bot
```

[`f1f3aa0`]: https://github.com/maastrichtlawtech/citation-enhance-merger/commit/f1f3aa0e386ccd9cec1d30f9976b6f04cc85eeb1