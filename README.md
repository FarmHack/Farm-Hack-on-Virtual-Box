# Farm-Hack-on-Virtual-Box
Use Vagrant for an easy sandbox setup of Farm Hack on Virtual Box

## Installation

```
## Get the FarmHack.org Sandbox repository
git clone https://github.com/FarmHack/Farm-Hack-on-Virtual-Box.git
cd Farm-Hack-on-Virtual-Box
## Download a FarmHack.org database dump and place in this folder as database.sql
cp ~/Downloads/farmhack.sql ./
## Build the sandbox
vagrant up
```

Then open your your web browser to http://127.0.0.1:8888/index.php
