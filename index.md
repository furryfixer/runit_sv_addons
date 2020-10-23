# runit-sv-addons
For those that are comfortable with basic "**sv...**" Runit commands, and do not need an entire new toolkit, here are three simple Bash scripts to reduce typing and lessen that chance of making a mistake while linking or unlinking runit services from "/etc/sv/" to "/var/service/".  Developed with Void Linux in mind.  The short scripts are self-explanatory.

##sv-add
```
#!/bin/bash
#
#  "sv-add" script by furryfixer which is glorified alias
#  for "ln -s /etc/sv/$1 /var/service/"
#  with an extra check to ensure operand is given and file exists

if [ -z "$1" ] || [ ! -z "$2" ]; then       
        echo "Syntax error. One and only one operand expected. "\""sv-add A"\"
        exit 1
fi
if [ ! -e "/etc/sv/$1" ]; then
	echo "Error. Service folder "\""/etc/sv/"$1\"" does not exist"
	exit 1
fi
ln -s /etc/sv/$1 /var/service/
exit
```
##sv-rm
```
#!/bin/bash
#
#  "sv-rm" script by furryfixer which is glorified alias
#  for "rm -r /var/service/$1"
#  There is an extra check to ensure operand is given and file exists
#  The service will be halted if possible before removal

if [ -z "$1" ] || [ ! -z "$2" ]; then       
        echo "Syntax error. One and only one operand expected. "\""sv-rm A"\"
        exit 1
fi
if [ ! -e "/var/service/$1" ]; then
	echo "Error. Service "\""/var/service/"$1\"" is already missing"
	exit 1
fi

sv d $1
rm -r /var/service/$1
exit
```
##sv-replace
```
#!/bin/bash
#
#  "sv-replace" script by furryfixer to replace one runit service with another.
#  Invoke in the form "sv-replace <old service> <new service>"
#  Useful for swapping login display managers such as XDM, LXDM, SDDM,
#  or network managers, etc.
#  Stops running service before replacing.
#  Service names must match entries in /etc/sv

if [ "$EUID" -ne 0 ]; then
	echo "You must be ROOT for this command to work"
	exit 1
fi
if [ -z "$2" ] || [ ! -z "$3" ]; then       
        echo "Syntax error. Two and only two operands expected. "\""sv-replace A B"\"
        exit 1
fi
if [ ! -e "/etc/sv/$1" ] || [ ! -e "/etc/sv/$2" ]; then
	echo "Syntax error. Usage is "\""sv-replace A B"\"", where A and B services both exist in /etc/sv"
	exit 1
fi
if [ ! -e "/var/service/$1" ]; then
	echo $1" service not linked in /var/service/. Try "\""sv-add "$2\"" instead."
	exit 1
fi
sv d $1
rm -r /var/service/$1
ln -s /etc/sv/$2 /var/service/
echo "Successfully replaced service "\"$1\"" with "\"$2\" 
exit
```

© 2020 GitHub, Inc.
Terms
Privacy
