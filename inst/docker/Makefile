#
# Makefile
# fbreitwieser, 2016-10-20 11:54
#

PORT=8080

build:
	docker build -t natay/pavian .

run:
	docker run --rm -p $(PORT):80 natay/pavian

push:
	docker push natay/pavian

inspect:
	docker run --rm -i -t --entrypoint /bin/bash -p $(PORT):80 natay/pavian

# vim:ft=make
#
