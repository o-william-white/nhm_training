Transferring files
==================

To upload and download data you can use various methods, such as scp, sftp, rsync, and samba/smb/cifs. Each method is described below. These instructions assume you are on VPN. If you are not, please see the orca user guide for instructions on how to transfer files through orca.

There are various ways of tranferring data to and from HPC, such as:

- :ref:`scp`
- :ref:`sftp`
- :ref:`rsync`
- :ref:`smb`

These methods are described below

.. _scp:

scp
---

You can use a graphical program like WinSCP, or you can use the command line, as described below.

To **upload** files or directories from your computer to the cluster::

    $ scp /path/to/source <username>@hpc-jobs-001:/path/to/destination

For example::

    $ scp -r myfiles/ robtest1234@hpc-jobs-001:~/mystuff
    robtest1234@hpc-jobs-001's password:
    file2                                                            100%    5     0.3KB/s   00:00
    file3                                                            100%    5     0.3KB/s   00:00
    file1                                                            100%    5     0.3KB/s   00:00


To **download** from the cluster to your computer::

$ scp -r <username>@hpc-jobs-001:/path/to/source /path/to/destination

For example::

    $ scp -r robtest1234@hpc-jobs-001:~/mystuff .
    robtest1234@hpc-jobs-001's password:
    file3                                                            100%    5     0.1KB/s   00:00
    file2                                                            100%    5     0.1KB/s   00:00
    file1                                                            100%    5     0.1KB/s   00:00

.. _sftp:    

sftp
----

You can use programs like FileZilla or Cyberduck, or you can use the command line, as described below.

To **upload** files or directories from your computer to the cluster, use the ``put`` option. For example::

    $ sftp robtest1234@hpc-jobs-001
    robtest1234@hpc-jobs-001's password:
    Connected to hpc-jobs-001.
    sftp> put -r myfiles/
    Uploading myfiles/ to /home/robtest1234/myfiles
    Entering myfiles/
    file2                                                            100%    5     0.3KB/s   00:00
    file3                                                            100%    5     0.3KB/s   00:00
    file1                                                            100%    5     0.2KB/s   00:00
    sftp> exit

To **download** files or directories from the cluster to your computer, use the ``get`` option. For example::

    $ sftp robtest1234@hpc-jobs-001
    robtest1234@hpc-jobs-001's password:
    Connected to hpc-jobs-001.
    sftp> get -r mystuff
    Fetching /home/robtest1234/mystuff/ to mystuff
    Retrieving /home/robtest1234/mystuff
    file3                                                            100%    5     0.1KB/s   00:00
    file2                                                            100%    5     0.1KB/s   00:00
    file1                                                            100%    5     0.1KB/s   00:00
    sftp> exit

.. _rsync:

rsync
-----

To **upload** files or directories from your computer to the cluster::

    $ rsync -avP /path/to/source <username>@hpc-jobs-001:/path/to/destination

For example::

    $ rsync -avP mystuff robtest1234@hpc-jobs-001:~/somedir
    -----------------------------------------------------------
    ---  This is the Natural History Museum in London, UK.  ---
    ---  If you are not an authorised user GO NO FURTHER !  ---
    ---  If you have problems connecting, contact :         ---
    ---          ts-servicedesk@nhm.ac.uk                   ---
    -----------------------------------------------------------
    robtest1234@hpc-jobs-001's password:
    sending incremental file list
    mystuff/
    mystuff/file1
                5 100%    0.00kB/s    0:00:00 (xfr#1, to-chk=2/4)
    mystuff/file2
                5 100%    4.88kB/s    0:00:00 (xfr#2, to-chk=1/4)
    mystuff/file3
                5 100%    4.88kB/s    0:00:00 (xfr#3, to-chk=0/4)

    sent 281 bytes  received 77 bytes  79.56 bytes/sec
    total size is 15  speedup is 0.04

To **download** files or directories from the cluster to your computer::

    $ rsync -avP  <username>@hpc-jobs-001:/path/to/source /path/to/destination

For example::

    $ rsync -avP robtest1234@hpc-jobs-001:~/somedir/mystuff .
    -----------------------------------------------------------
    ---  This is the Natural History Museum in London, UK.  ---
    ---  If you are not an authorised user GO NO FURTHER !  ---
    ---  If you have problems connecting, contact :         ---
    ---          ts-servicedesk@nhm.ac.uk                   ---
    -----------------------------------------------------------
    robtest1234@hpc-jobs-001's password:
    receiving incremental file list
    mystuff/
    mystuff/file1
                5 100%    4.88kB/s    0:00:00 (xfr#1, to-chk=2/4)
    mystuff/file2
                5 100%    4.88kB/s    0:00:00 (xfr#2, to-chk=1/4)
    mystuff/file3
                5 100%    4.88kB/s    0:00:00 (xfr#3, to-chk=0/4)

    sent 85 bytes  received 309 bytes  87.56 bytes/sec
    total size is 15  speedup is 0.04

.. _smb:    

smb
---

You can transfer files via samba (also known as SMB, or CIFS) via File Explorer in Windows or Finder in macOS. 

To do this via File Explorer in Windows, browse to the location of the workspace: ``\\valentine\mbl\share\workspaces\groups\<folder_name>``

.. image:: _images/file-explorer-smb.png

Or you can use the command line, as described below. You may need to install smbclient first. On Ubuntu Linux you can do this with the following command::

$ sudo apt install smbclient

Upload files
^^^^^^^^^^^^

To upload a file, connect to Valentine via smbclient::

$ smbclient -U <username>@nhm.ac.uk \\\\valentine\\share

Change to the directory you want to upload the file to::

$ cd <destination_folder>

Upload the file::

$ put myfile

Check that the file has been uploaded::

$ ls

Exit the prompt::

$ exit

.. image:: _images/terminal-smbclient-put.png

Download files
^^^^^^^^^^^^^^

To download a file, connect to Valentine using smbclient::

$ smbclient -U <username>@nhm.ac.uk \\\\valentine\\share

Change to the directory you want to download the file from::

$ cd <origin_folder>

Download the file::

$ get myfile

Check that the file is now in your local directory::

$ ls

Exit the prompt::

$ exit

.. image:: _images/terminal-smbclient-get.png



