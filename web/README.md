> This file contains a Markdown version of the four web pages in this folder. The HTML pages retrieved from the Wayback Machine's archive of the original site at [home.roadrunner.com/~ernieyu/pfcviewer](https://web.archive.org/web/20140531053212/http://home.roadrunner.com/~ernieyu/pfcviewer/), and then cleaned up and merged into this single `.md` file.


# PFC Viewer

PFC Viewer is a Java application I developed to view and export the
contents of the AOL Filing Cabinet, also known as the Personal Filing
Cabinet.  I have had an AOL account for many years, and even though my
primary Internet account is now with the cable company providing my
high-speed access, I still keep the old account around for a variety of
minor reasons.  One nagging issue is the accumulation of old emails
archived within my Personal Filing Cabinet.  To maintain access to years
of correspondence, I need to use the AOL client program because nothing
else can read their proprietary file format for the cabinet.  Now, with
some software handiwork, I can open the cabinet file, display its
contents in a window, and export the emails to a text file.

What PFC Viewer can do:

- Display the folder structure in the Filing Cabinet.
- Display the list of emails stored in a folder, and the content of a
  selected email.
- Copy selected email text to the system clipboard.
- Export emails in a selected folder, and its sub-folders, to a text
  file compatible with other email programs.
- Export favorites to an HTML file.

What PFC Viewer cannot do:

- Export address book entries in the Filing Cabinet.
- Repair a corrupt Filing Cabinet.
- Cure cancer.

PFC Viewer would not have been possible without the ground-breaking work
done by another AOL user, Franz von Gordon, to analyze and document the
AOL Filing Cabinet file format.  His work can be found
[here](https://web.archive.org/web/20131106051712/http://members.aol.com/fvongordon/pfc/).

## Download

Disclaimer: While every reasonable effort has been made to provide a
safe computing experience, all risks associated with downloading and
operating this application are assumed by you.  If something bad happens
because you didn\'t understand what was going on, or even if the program
contained an error, any damage is not my fault.  If you cannot deal with
these concepts, do not try to use this program.

### Requirements

You need a recent version of the Java Runtime Environment (JRE) to run
PFC Viewer.  On a Windows machine, you can tell what version of the JRE
you have by opening a Command Prompt (or MS-DOS Prompt) window, and
running the command:

```sh
java -version
```

The application requires version 1.4 or later; it will not work with
earlier versions.  If you need to, you can download the JRE from
[Sun](http://java.sun.com/j2se/downloads.html).  Note that you only need
to install the JRE to run the application, not the whole SDK.  Be sure
to follow the installation instructions; you should uninstall previous
versions of the JRE before installing the new one.

Aside from that, PFC Viewer should work on any machine running Windows
98 or later, as well as Linux.

### Download

To obtain the PFC Viewer application, download the **pfcview.jar** file
containing the application classes.  To install, save the file using the
.jar extension into a directory such as `C:\My Documents\Pfcviewer`,
or onto the Windows Desktop.


## Operation

The **pfcview.jar** file actually contains two applications.  The
primary one is [PFC Viewer](#pfcviewer), which is a Java Swing GUI
application with windows and menus and stuff.  The other one is a simple
console application named [MailToMbox](#mailtombox).  When running these
applications, note that all Java commands are case-sensitive.

### [PfcViewer]{#pfcviewer}

To start the PFC Viewer application in Windows, select Run from the
Start menu, browse to the **pfcview.jar** file, and press OK.  If you
saved the file to the Windows Desktop, you can simply double-click the
file.

Alternatively, you can open a Command Prompt (or MS-DOS Prompt) window,
navigate to the directory where you saved the pfcview.jar file, and run
the following command:

```sh
java -jar pfcview.jar
```

The main application window is shown on the right.  To open your Filing
Cabinet, select **File Open** from the menu and use the dialog box to
locate your cabinet file.  (This is usually stored in the \"organize\"
sub-folder where the AOL software is installed.  The file is named after
your screen name, without an extension, like \"JohnDoe123\".)  *It is
strongly recommended that you copy the file from its original location
to another separate directory, and work with the copy.*

Once the cabinet file is loaded, you will see its folder structure in
the left-side window.  Select any folder, and its contents will be
listed in the upper right-side window.  Select any mail message in the
table, and its text will be displayed in the lower right-side window.

There are a few options that affect the text display; these can be
accessed using the **Tools Options** dialog box.

- **Append mail headers in Text view**: When checked, mail headers are
  appended to the end of the mail body in the display window.  This
  setting is ignored for HTML mail in the Browser view.
   
- **Use Browser view for HTML mail**: When checked, HTML mail is
  displayed in browser mode, which interprets all tags and formats the
  content.  This feature is still a little flaky so some messages may
  not display properly.  Text mail is displayed in plain text.

Exporting items to a text file is a matter of selecting a folder, and
choosing one of the export options from the menu.  You may select any
folder, including the root folder identified by the AOL screen name at
the top.  The current export options are listed below.

- **Tools Export Favorites to Html**: Exports favorites in the selected
  folder, and its sub-folders, to an HTML file.  This is a simple way of
  exporting the favorite URLs into a format that can be referenced by
  any web browser.  The folder structure is reflected in the file.
   
- **Tools Export Mail to Mbox**: Exports emails in the selected folder,
  and its sub-folders, to an [mbox](faq.html#1.3) file.  This is a
  standard Unix mail file format, which is also used by many Windows
  mail clients like Eudora, Mozilla, and Netscape.  The mbox file is a
  text file, so it can also be opened in any plain text editor.  The
  folder structure is not preserved in the file.
   
- **Tools Export Mail to Mbox+Toc**: Exports emails in the selected
  folder, and its sub-folders, to an [mbox](faq.html#1.3) file, and
  generates Eudora index files.  There are two index files: one is named
  \<mailbox\>.toc, and the other is named descmap.pce.  The folder
  structure is not preserved.  Use this function to generate files that
  may be imported into Outlook Express.

The remaining menu selections are straight-forward.

- **File Open**: Opens the Filing Cabinet, and loads the folder
  hierarchy into the left-side window.
- **File Close**: Closes the current cabinet.
- **File Exit**: Exits the application.
- **Edit Copy**: Copies the selected text to the clipboard.
- **Tools About**: Displays the version number and date of the
  application.

### Startup Options

There is one optional parameter when starting the application.  You can
supply the cabinet path and file name on the command line, and the main
window will open and immediately load the cabinet file.

```sh
java -jar pfcview.jar [pfcfile]
```

You can also use the `javaw` command to start the application.  This
command does not require a Command Window to run, which makes it
convenient if you are setting up a Windows shortcut to start the
application.  In the shortcut properties, enter the following settings:

```
Target:   javaw -jar pfcview.jar [pfcfile]
Start in: [directory path for pfcview.jar] 
```


### [MailToMbox]{#mailtombox}

The MailToMbox application performs a simple export of all mail messages
in the Filing Cabinet to a single mbox file.  The folder structure of
the cabinet is completely ignored, which makes the program useful if PFC
Viewer is unable to read or interpret the internal pointers defining the
file structure.  (This may happen if the cabinet file has not been
compacted in a long time, or if the file is slightly corrupted.)

To run the MailToMbox application in Windows, open a Command Prompt (or
MS-DOS Prompt) window, navigate to the directory where you saved the
pfcview.jar file, and run the following command:

```sh
java -cp pfcview.jar MailToMbox [pfcfile] [mboxfile].mbx
```

The suggested file extension for the destination mbox file is `.mbx`,
which is used by Eudora for Windows.  MailToMbox is strictly a console
application, so no windows will appear.  The program will report its
results in the command window when the export is finished.


## FAQ

* [Which versions of AOL does the PFC Viewer work for?](#1.1)
* [Does PFC Viewer alter my Filing Cabinet in any way?](#1.2)
* [What can I do with an mbox file?](#1.3)
* [How can I import my emails into Outlook Express?](#1.4)
* [Can I view or export Address Book entries in the Filing Cabinet?](#1.5)
* [Where can I get more help?](#1.6)
* [What tools did you use to develop the PFC Viewer?](#1.7)
* [Is the source code available for PFC Viewer?](#1.8)
* [What is the history of the PFC Viewer application?](#1.9)

[Which versions of AOL does the PFC Viewer work for?]{#1.1}
: I have successfully opened cabinet files from AOL 5, 7, and 9.  I have
  received reports that AOL 8 cabinet files can also be opened.

[Does PFC Viewer alter my Filing Cabinet in any way?]{#1.2}
: No.  PFC Viewer is designed to open the cabinet file read-only, load
  the contents into memory, and immediately close the file.  Which
  doesn\'t mean that something bad couldn\'t happen.  I don\'t mean to
  sound like a doomsayer, but computers have been known to crash from
  time to time with unpredictable results, and people have been known to
  choose an export file name identical to the original cabinet file
  name.  *So it is still best practice to copy your cabinet file to
  another separate directory, and work with the copy*.

[What can I do with an mbox file?]{#1.3}
: You can import it into a variety of other mail clients - Eudora,
  Netscape, Outlook Express, and Thunderbird to name a few - on a
  variety of platforms.  You can also open an mbox file in a plain text
  editor.  This is as close to a universal mail file format as I could
  find.  It is a very simple one to develop for, so it made my life much
  easier.  The format for mail messages is described in [RFC
  822](https://www.rfc-editor.org/rfc/rfc822.txt). More information
  about the mbox file format can be found
  [here](https://web.archive.org/web/20131022025832/http://www.qmail.org/qmail-manual-html/man5/mbox.html).

[How can I import my emails into Outlook Express?]{#1.4}
: Use the **Export Mail to Mbox+Toc** function to generate a set of
  mailbox files that can be imported into Outlook Express 5.  To import
  the messages into OE, select the File Import Messages menu item.  When
  prompted by the import wizard, choose the Eudora Pro or Light program,
  browse to the directory where you exported your emails, and select the
  mailbox file to import.  With luck, the process should work smoothly.

[Can I view or export Address Book entries in the Filing Cabinet?]{#1.5}
: You can only view AOL 5 Address Book entries; no export function is
  available at this time.  Note that starting with AOL 6, address book
  entries are stored in a separate set of files with the extensions
  .ab\*.  Starting with AOL 7, address book entries are encoded using an
  undetermined method.  I have not embarked on the task of reading these
  files, decoding their entries, and combining them into the display,
  and I\'m not sure if or when I ever will.

[Where can I get more help?]{#1.6}
: While suggestions are welcome, writing to me for assistance will prove
  to be unreliable.  Don\'t take it personally; it\'s a sign of the
  times.  I am only one man, and it\'s unlikely I\'ll be able to respond
  to every request that comes my way.  In addition, I may confuse your
  email with the endless rivers of spam that collects in my Inbox, and
  delete it.  Instead, try writing to one of the **newsgroups** about
  AOL, like
  [alt.aol.tricks](https://web.archive.org/web/20131022025832/news://alt.aol.tricks/)
  or
  [alt.online-service.america-online](https://web.archive.org/web/20131022025832/news://alt.online-service.america-online/).
  I look there from time to time, so you might even get a response from
  your friendly, neighborhood software developer.  If I\'m in the mood.

[What tools did you use to develop the PFC Viewer?]{#1.7}
: The PFC Viewer was originally developed using the [Sun
  Java](https://web.archive.org/web/20131022025832/http://java.sun.com/)
  2 Software Development Kit (J2SDK) v1.5.0.  To create the Java Swing
  user interface, I used the
  [NetBeans](https://web.archive.org/web/20131022025832/http://www.netbeans.org/)
  v5.0 IDE - this is the open-source variant of the old Sun Forte for
  Java IDE (now known as Sun ONE Studio).  [Apache
  Ant](https://web.archive.org/web/20131022025832/http://ant.apache.org/)
  1.6.2 was used to build the project.  The development work was done
  under Linux, while final testing was done under Windows.

[Is the source code available for PFC Viewer?]{#1.8}
: Yes.  The source code for PFC Viewer is hosted on
  [GitHub](https://web.archive.org/web/20131022025832/https://github.com/ernieyu/pfcviewer). 
  The code is distributed under the terms of the [MIT
  license](https://web.archive.org/web/20131022025832/http://www.opensource.org/licenses/mit-license.php). 
  The source code for the current version is also available in the
  archive file
  [pfcview-0.5-src.jar](https://web.archive.org/web/20131022025832/http://home.roadrunner.com/~ernieyu/pfcviewer/pfcview-0.5-src.jar).

[What is the history of the PFC Viewer application?]{#1.9}
: The brief version history of the PFC Viewer application can be found in the `releases.txt` file at the root of this repository.

## Donation

PFC Viewer is freeware.  You are under no obligation to pay for its
download and use.  I developed the application on my own time and for my
own purposes, and I made it available because I thought others might
find it useful.  There are no plans to license the program, or expand
the project to include professional services like automated
installation, upgrades, bug fixes, or support.  I may work on it some
more, or I may not.  Hence, no fees are appropriate or required.

However, I accept donations.  No services are offered in return, either
in the present or in the future, and you will not be notified if my
policies change.  My only promise is to pass on further goodwill to
other souls on this planet as my abilities and resources allow.

So if you found PFC Viewer useful to your ends, and you accept the
conditions noted above, you may contribute to my PayPal business account
by pressing the button below.

> The donation button has been removed because the site is no longer live.

Note: Donations are not tax-deductible.

Peace to all,

Ernie Yu

--- 

Copyright © 2002-2007 Ernie Yu.
