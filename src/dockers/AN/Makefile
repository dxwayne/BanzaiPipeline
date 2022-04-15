#############################################################################
# Make the test environment and other things.
#############################################################################


all: tmpfiles

base:
	(cd base && docker build -t astrometrynet/base:latest . )

solver:
	(cd solver && docker build -t astrometrynet/solver:latest  --no-cache .)
	
webserver:
	(cd webservice && docker build -t astrometrynet/webserver:latest .)



tmpfiles: 
	mkdir -p /tmp/nova/build
