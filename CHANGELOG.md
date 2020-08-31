# Changelog

All notable changes to this project will be documented in this file.

## 31-08-2020

### [`f8a918f`] Change ECHR_metadata_harvester to merge json results

- Changed to relative paths. Got rid of path basename joining.

- `get_echr_metadata` in [`ECHR_metadata_harvester.ipynb`](./notebooks/ECHR_metadata_harvester.ipynb) is extracting all the JSON Objects (`dict`s) from `columns` field (inside HUDOC API's response). It appends them and exports as JSON. 

### [`abf3a11`] Add citations diff. Add language merge. *WIP* dataset merge 

- **Preprocessing**: Remove duplicated application numbers in `exractedappno`.
  
  `exractedappno` starts with the `appno` of the current case. Got rid of it to avoid redundancy, _eg_ `appno=38042/06 extractedappno=38042/06;30979/96;31932/03` becomes `appno=38042/06 extractedappno=30979/96;31932/03`
  
- **Citations diff** _(scl vs extractedappno)_

  Explore how frequently citations are missing when comparing the two columns in the HUDOC-metadata. Calculate whether the number of citations in the scl column exceeds the number of citations in the extractedappno column or v.v.
  
  Created a `citation_diff_extr_vs_scl_all` column representing the weights as it follows:

    - If number of citations in extractedappno **>** number of citations in scl: **score=1**
    - If number of citations in extractedappno **<** number of citations in scl: **score=-1**
    - If number of citations in extractedappno **=** number of citations in scl: **score=0** 
    
- **Language merge**

  For each application number `appno`, merge the application numbers from `extractedappno` (for the different languages) column.
  
  Group all the rows with same `appno`, merge the `extractedappno` lists in one and apply it to all the relevant rows.


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
[`f8a918f`]: https://github.com/maastrichtlawtech/citation-enhance-merger/commit/f8a918f300d6feb5078988125dc48b158283f686
[`abf3a11`]: https://github.com/maastrichtlawtech/citation-enhance-merger/commit/abf3a1196a90bf142f16ac6d27066670c3365894
