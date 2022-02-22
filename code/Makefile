SERVICE_NAME = $(shell basename hot-raml)

OS = $(shell uname -s | tr A-Z a-z)
ARCH = $(shell uname -m | tr A-Z a-z)

ZIP_DATE = $(shell date +"%Y%m%d.%H%M%S")

SERVICE_VERSION = $(shell cat VERSION)
ZIP_NAME = $(SERVICE_NAME)-$(SERVICE_VERSION)

SEDFLAG = --transform=s/runtime/$(ZIP_NAME)/g
ifeq ($(OS),darwin)
	SEDFLAG = -s /runtime/$(ZIP_NAME)/g
endif

all: release

build:
	cd ../go-bindata && go install ./...
	cd ../go-raml && make install
	cd ../hot && make install
	cp ~/go/bin/go-bindata ../runtime
	cp ~/go/bin/go-raml ../runtime
	cp ~/go/bin/hot ../runtime

zip: build
	cd .. && tar -zcvf dist/$(ZIP_NAME).$(ZIP_DATE).tgz $(SEDFLAG) --exclude .gitignore runtime

release: build
	cd .. && tar -zcvf dist/$(ZIP_NAME).$(OS).$(ARCH).tgz $(SEDFLAG) --exclude .gitignore runtime
