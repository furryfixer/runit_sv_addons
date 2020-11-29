# runit-sv-addons
For those that are comfortable with basic "**sv...**" Runit commands, and do not need an entire new toolkit, here are three simple Bash scripts to reduce typing and lessen that chance of making a mistake while linking or unlinking runit services from "/etc/sv/" to "/var/service/".  Developed with Void Linux in mind.  The short scripts are self-explanatory.

## sv-add
	A glorified alias for "ln -s /etc/sv/$1 /var/service/", with an extra check to ensure operand is given and file exists.
	
## sv-rm
	A glorified alias for "rm -r /var/service/$1". There is an extra check to ensure operand is given and file exists. The service will be halted if possible before removal

## sv-replace
	A script to replace one runit service with another. Stops running service before replacing.

© 2020 GitHub, Inc.
Terms
Privacy
