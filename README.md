This is not my work! This is Ernest Yu's [PFC Viewer](https://github.com/ernieyu/pfcviewer), with some minor modernization (dealing with warnings from old code), plus a few fixes for parsing dates and times from email headers.

The original home page for the project no longer exists, but you can find an archived version of it in the [`web`](web) folder of this repo.

_— Ben Fry, 19 October 2026_


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


# File Format 

You can read Franz v. Gordon's documentation of the PFC file format [here](format.md).
