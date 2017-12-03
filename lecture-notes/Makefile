VERSION=0.9-prerelease

TARGETS=linux-command.html devel-tools.html raspberrypi.html raspberrypi-camera.html

html: $(TARGETS)

all: html

%.html : %.md boilerplate.html
	sed -e '/<textarea id="source">/r $<' boilerplate.html > $@

clean:
	rm $(TARGETS) 

