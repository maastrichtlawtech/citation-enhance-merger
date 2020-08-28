# Changelog

All notable changes to this project will be documented in this file.

## 29-08-2020

### [`f1f3aa0`] Add workflow to update the requirements for python 3.7

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

### [`e7b5e35`] Import legacy code. Minor changes

- Moved paths to constants. Moved absolute paths to relatives paths

  Assuming the `data` directory is placed in the same directory with the `notebooks`, considered changing to relative paths for the JSON files.

  ```python
  ECHR_DATA_FILE = '../data/ECHR_metadata_0till600.json'
  NETWORK_FILE = '../data/network.json'
  ```

- List comprehension for columns filed

  Squashed

  ```python
  for k in data:
    v = k.get('columns')
    list_of_dicts.append(v)
  ```

  into the following one-liner

  ```python
  list_of_dicts = [k.get('columns') for k in data]
  ```

  Read more about [Python List Comprehension](https://www.programiz.com/python-programming/list-comprehension).

[`f1f3aa0`]: https://github.com/maastrichtlawtech/citation-enhance-merger/commit/f1f3aa0e386ccd9cec1d30f9976b6f04cc85eeb1
[`e7b5e35`]: https://github.com/maastrichtlawtech/citation-enhance-merger/commit/e7b5e355d040e00d3be524f903f49c15b376ca43
