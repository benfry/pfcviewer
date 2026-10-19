Original project page no longer exists; but here is [home.roadrunner.com/~ernieyu/pfcviewer](https://web.archive.org/web/20140531053212/http://home.roadrunner.com/~ernieyu/pfcviewer/) from the Wayback Machine.


# Build

To (re)build the project:

```sh
ant clean dist
```


# Run

To run the application:

```sh
java -jar dist/pfcview.jar
```

To open an AOL Filing Cabinet, select File Open from the menu.


# Mailbox

To convert a single mailbox file:

```sh
java -cp build MailToMbox username username.mbx
```

Convert several, inside a folder:

```sh
find ~/Desktop/aol/ -type f -exec java -cp build MailToMbox {} {}.mbx ';'
```
