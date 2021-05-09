Checkgmail
Copyright &copy; 2005-6, Owen Marshall


About
-----

CheckGmail is a *nix system tray application that checks a Gmail account for new mail. It is fast, secure and uses minimal bandwidth via the use of atom feeds.

When new mail is present the tray icon changes and a popup window displays the number and details of new messages. Configuration is GUI-based and the application is designed to be simple, elegant and unobtrusive.


Why?
----

There are several other gmail-notify alternatives around (see the links section below), but all use a cumbersome method of gathering Gmail account information that basically involves loading the login page, logging in just as a web-browser would, then parsing the html received and mining the relevant data. The advantage of this approach is that you can grab any information that you can see on the Gmail web interface. The disadvantage is that it uses heaps of bandwidth and time when typically all you want to know is whether there's new mail in your account. In comparison, the use of atom feeds by CheckGmail is simple, straightforward and fast while still providing the details of any new mail in your inbox.

The other reason for CheckGmail is security - no password information should ever be stored in plain text, yet this is exactly what at least one popular alternative does. CheckGmail provides the option of either encrypting the saved password information or - for maximum security - re-entering your password each time the application is run. If you decide to save the password, it is encrypted using a passphrase generated from machine-unique information (the eth0 MAC address and/or uname system information). Encrypting the password prevents both casual reading of plain text passwords on your machine, but more importantly allows the CheckGmail config file in your home directory to be stored securely on external backups.


Requirements / Installation
---------------------------

CheckGmail requires the following Perl modules to run:

	Gtk2-perl
	Gtk2-TrayIcon
	libwww-perl
	Crypt-SSLeay
	XML-Simple

In addition, for the ability to save encrypted passwords, you'll need these modules:

	Crypt-Simple
	Crypt-Blowfish
	FreezeThaw
	Compress-Zlib
	Digest-MD5
	MIME-Base64

And to be able to use custom icons for the mail status, you'll need:

	Gtk >= 2.18
	Gtk2-Perl bindings updated to Gtk-2.18

Ubuntu users (and others) may find the following web HOWTO useful, which details and simplifies the install process nicely:

http://ubuntuforums.org/printthread.php?t=60418

Note that if you run CheckGmail from the commandline, it will print out a list of required modules not present on your system.

As of v0.9.5, the default mail images have now been embedded in the script, so you can happily install CheckGmail anywhere on your system.  You probably want to stick it in a directory in your `$PATH` ...

CheckGmail stores the user settings in the file `.checkgmail/prefs` in your home directory.  This is a simple text file, and any of the values (such as the tray background colour) can be changed by editing this file, as well as by using the nice GUI Preferences dialogue.


Contact
-------

Contact the author at

owenjm@users.sourceforge.net

Suggestions, feedback, further ideas on password protection and security, feature requests and bug reports are welcome.
