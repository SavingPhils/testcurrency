
Debian
====================
This directory contains files used to package testcurrencyd/testcurrency-qt
for Debian-based Linux systems. If you compile testcurrencyd/testcurrency-qt yourself, there are some useful files here.

## testcurrency: URI support ##


testcurrency-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install testcurrency-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your testcurrencyqt binary to `/usr/bin`
and the `../../share/pixmaps/testcurrency128.png` to `/usr/share/pixmaps`

testcurrency-qt.protocol (KDE)

