===================
Sphinx-PDF Generate
===================

Build HTML and PDF files of your Sphinx documentation and transfer the build files to a target branch.
The target branch contents can be copied to any static site hosting platform such as GitHub Pages.

Usage
=====

This action only helps you build and commit HTML and PDF files of your Sphinx documentation to ``target_branch``,
branch. 

Also we need to use some other actions:

- ``action/setup-python@v3`` for installing Python and Pip
- ``actions/checkout`` for checking out git repository
- ``ad-m/github-push-action`` for pushing static build files to remote target branch

For example, you can create a YAML file for your GitHub workflow with the following contents 
and save it under the ``.github/workflows`` directory in your repository:

.. code-block:: yaml

   name: Sphinx-PDF Generate Deploy
   on:
     push:
       branches:
       - main
   jobs:
     build:
       runs-on: ubuntu-latest
       steps:
       - uses: actions/setup-python@v3
         with:
           python-version: 3.11
       - uses: actions/checkout@master
         with:
           fetch-depth: 0 # otherwise, you will failed to push refs to dest repo
       - name: Build and Commit
         uses: iSOLveIT/sphinx-pdf-generate-action@main
         with:
           config_file_path: docs/conf.py
           target_branch: 'gh-pages' 
       - name: Push changes
         uses: ad-m/github-push-action@master
         with:
           github_token: ${{ secrets.GITHUB_TOKEN }}
           branch: site

Inputs
======

======================= ================ ============ ===============================
Input                   Default          Required     Description
----------------------- ---------------- ------------ -------------------------------

``config_file_path``    ``'conf.py'``    ``true``     Relative path to the 
                                                      `conf.py` configuration 
                                                      file
``documentation_path``  ``'./docs'``     ``false``    Relative path under
                                                      repository to documentation
                                                      source files
``target_branch``       ``'gh-pages'``   ``false``    Git branch where assets will
                                                      be deployed
``target_path``          ``'.'``         ``false``    Path in ``$target_branch``
                                                      where Sphinx build files will be
                                                      placed
``repository_path``     ``'.'``          ``false``    Relative path under
                                                      ``$GITHUB_WORKSPACE`` to
                                                      place the repository.
                                                      You do not need to set this
                                                      Input unless you checkout
                                                      the repository to a custom
                                                      path
``requirements_path``   ``'.'``          ``false``    Relative path under
                                                      ``$repository_path`` to pip
                                                      requirements file
``sphinx_version``      ``'5.3'``        ``false``    Custom version of Sphinx
======================= ================ ============ ===============================
