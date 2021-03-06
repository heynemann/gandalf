API Reference
=============

User creation
-------------

Creates an user in the database.

* Method: POST
* URI: /user
* Format: json

User removal
------------

Removes an user from the database.

Key add
-------

Adds a key to an user in the database and writes it in authorized_keys file from the user running Gandalf.

Key removal
-----------

Removes a key from a user in the database and from the authorized_keys file from the user running Gandalf.

Repository creation
-------------------

Creates a repository in the database and an equivalent bare repository in the filesystem.

Repository removal
------------------

Removes a repository from the database and the equivalent bare repository from the filesystem.

Repository retrieval
--------------------

Retrieves information about a repository.

Access grant in repository
--------------------------

Grants an user read and write access into a repository.

Access revoke in repository
---------------------------

Revokes an user read and write access from a repository.

Get file contents
-----------------

Returns the contents for a `path` in the specified `repository` with the given `ref` (commit, tag or branch).

* Method: GET
* URI: /repository/`:name`/contents?ref=:ref&path=:path
* Format: binary

Where:

* `:name` is the name of the repository;
* `:path` is the file path in the repository file system;
* `:ref` is the repository ref (commit, tag or branch). **This is optional**. If not passed this is assumed to be "master".

Example URLs (http://gandalf-server omitted for clarity)::

    $ curl /repository/myrepository/contents?ref=0.1.0&path=/some/path/in/the/repo.txt
    $ curl /repository/myrepository/contents?path=/some/path/in/the/repo.txt  # gets master

Get tree
--------

Returns a list of all the files under a `path` in the specified `repository` with the given `ref` (commit, tag or branch).

* Method: GET
* URI: /repository/`:name`/tree?ref=:ref&path=:path
* Format: JSON

Where:

* `:name` is the name of the repository;
* `:path` is the file path in the repository file system. **This is optional**. If not passed this is assumed to be ".";
* `:ref` is the repository ref (commit, tag or branch). **This is optional**. If not passed this is assumed to be "master".

Example result::

    [{
        filetype: "blob",
        hash: "6767b5de5943632e47cb6f8bf5b2147bc0be5cf8",
        path: ".gitignore",
        permission: "100644",
        rawPath: ".gitignore"
    }, {
        filetype: "blob",
        hash: "fbd8b6db62282a8402a4fc5503e9a886b4fb8b4b",
        path: ".travis.yml",
        permission: "100644",
        rawPath: ".travis.yml"
    }]

`rawPath` contains exactly the value returned from git (with escaped characters, quotes, etc), while `path` is somewhat cleaner (spaces removed, quotes removed from the left and right).

Example URLs (http://gandalf-server omitted for clarity)::

    $ curl /repository/myrepository/tree                                 # gets master and root path(.)
    $ curl /repository/myrepository/tree?ref=0.1.0                       # gets 0.1.0 tag and root path(.)
    $ curl /repository/myrepository/tree?ref=0.1.0&path=/myrepository    # gets 0.1.0 tag and files under /myrepository

Get archive
-----------

Returns the compressed archive for the specified `repository` with the given `ref` (commit, tag or branch).

* Method: GET
* URI: /repository/`:name`/archive/`:ref.:format`
* Format: binary

Where:

* `:name` is the name of the repository;
* `:ref` is the repository ref (commit, tag or branch);
* `:format` is the format to return the archive. This can be zip, tar or tar.gz.

Example URLs (http://gandalf-server omitted for clarity)::

    $ curl /repository/myrepository/archive/master.zip        # gets master and zip format
    $ curl /repository/myrepository/archive/master.tar.gz     # gets master and tar.gz format
    $ curl /repository/myrepository/archive/0.1.0.zip         # gets 0.1.0 tag and zip format
