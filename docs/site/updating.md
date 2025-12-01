# Updating the site

The K23OJ is under active development, so occasionally you may wish to update. This is a fairly simple process.

!>  The K23OJ development team makes no commitment to backwards compatibility. It's possible that an update migration
    might add, change, or delete data from your install. Always back up before attempting an update. <br> <br>
    If in doubt, feel free to [contact us on Discord](https://discord.gg/TDyYVyd).

First, switch to the site virtual environment, and pull the latest changes.

```
(k23ojsite) $ git pull origin master
```

Dependencies may have changed since the last time you updated, so install any missing ones now.

```
(k23ojsite) $ pip3 install -r requirements.txt
```

The database schema might also have changed, so update it.

```
(k23ojsite) $ ./manage.py migrate
(k23ojsite) $ ./manage.py check
```

Finally, update any static files that may have changed.

```
(k23ojsite) $ ./make_style.sh
(k23ojsite) $ ./manage.py collectstatic
(k23ojsite) $ ./manage.py compilemessages
(k23ojsite) $ ./manage.py compilejsi18n
```

That's it! You may wish to condense the above steps into a script you can run at a later time.
