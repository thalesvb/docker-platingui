=======================
PlatinGUI inside Docker
=======================

This repository contains dockerfiles to install PlatinGUI (SAPGUI for Unix) inside Docker container.
The installer isn't redistributable, you must download it from MarketPlace and put on same folder of Dockerfile file before creating the image.

What I need?
============

This repository (and guide) consider you have the PlatinGUI installer in your hands and your system have X11.

How to build?
=============

Open a terminal with root user, go to folder with dockerfile for your PlatinGUI version, copy the installer in that folder, and run the following command:

``# docker build -t="thalesvb/platingui" .``

And now?
========

Run on terminal:

``# docker run -v /tmp/.X11-unix:/tmp/.X11-unix --rm -e DISPLAY "thalesvb/platingui"``

and PlatinGUI will start.
The line above will not save the container's state (each time it will run as if you have just finished install PlatinGUI).
If you want persist the changes, please read the Docker docs to know how use it better.

Tip: create an entry in your system launcher menu with that command, so you won't need to open terminal each time you want start PlatinGUI.
