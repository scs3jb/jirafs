Getting Started
===============

Installation
------------

It is recommended that you install the program using ``pip`` while in a
Python 3 virtualenv;  you can install using ``pip`` by running::

    pip install jirafs

After Jirafs successfully installs, you'll have access to the ``jirafs``
command that you can use for interacting with JIRA.

Working with a JIRA issue
-------------------------

First, you'll need to "clone" the issue you want to work with using
Jirafs by running the following
(replacing ``http://my.jira.server/browse/MYISSUE-1024`` with the
issue url you are concerned about)::

    jirafs clone http://my.jira.server/browse/MYISSUE-1024

The first time you run this command, Jirafs will ask you for a series of details
that it will use for communicating with JIRA; don't worry: although all of this
information will be stored in a plaintext file at ``~/.jirafs_config``, Jirafs will
not store your password unless you give it permission to do so.

Once the command runs successfully, it will have created a new folder named
after the issue you've cloned, and inside that folder it will place a series of
text files representing the issue's contents in JIRA as well as copies of
all attachments currently attached to the issue in JIRA.

The following text files are created:

* ``fields.jira``:  This file will show all currently-set field values
  for this JIRA issue (except fields written to their own files; see
  ``description.jira`` below).  You **can** change field values here
  by editing the field values in the file.
* ``description.jira``: This file will show the issue's current
  description.  You **can** change the issue's description by editing
  the contents of this file.
* ``comments.read_only.jira``: This file shows all comments currently
  posted to this issue.  Note that you **cannot** edit the comments in
  this file.
* ``new_comment.jira``: This file starts out empty, but if you would
  like to add a new comment, you **can** create one by entering text
  into this file.

.. note::

   For many fields from JIRA, the incoming data is a JSON dictionary
   or list.  If you see fields having values starting with either a
   "[" or "{", you may need to consult with JIRA's documentation to
   develop an understanding of how to change those values.

In order to update any of the above data or upload an asset, either
make the change to a field in ``fields.jira``, edit the issue's
description in ``description.jira``, write a comment into
``new_comment.jira``, or copy a new asset into this folder, then run::

    jirafs commit

from within the folder ``jirafs`` created earlier.

.. note::

   Unlike when working with a git repository, you do not need to 'stage' 
   files using a command analogous to git's "add" command when working with
   a JIRA issue using Jirafs.  All uncommitted files will
   automatically be included in any commit made.  This behavior will likely
   be surprising only to users who have not worked with mercurial (hg).

Running this command will mark the changes you've made as ready for
submission to JIRA.  At any time, you can run::

    jirafs status

to see both what changes you've marked as ready for being submitted
to JIRA as well as which changes you have made, but not yet committed.

Once you're satisfied with the changes that are about to be submitted to
JIRA, run::

    jirafs push

Please keep in mind that updates that others have made in JIRA outside of 
Jirafs won't be available in your local copy until you pull them in by
running::

    jirafs pull

Please consider the above to be just a simple overview -- there are a
variety of other commands you can run to have finer-grained control
over how the issue folder is synchronized with JIRA; see :doc:`commands`
for more details.

.. note::

   If you are a VIM user, there is a :ref:`vim-plugin`  available that provides
   syntax highlighting for JIRA/Confluence's wikimarkup.

