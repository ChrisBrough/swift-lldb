LEVEL = ../../../../make
SWIFT_OBJC_INTEROP := 1

# This test builds an Objective-C main program that imports two Swift
# .dylibs with conflicting ClangImporter search paths.

include $(LEVEL)/Makefile.rules

a.out: main.m libFoo.dylib
	$(CC) $(CFLAGS) $(MANDATORY_MODULE_BUILD_CFLAGS) -I$(shell pwd) -lFoo -L$(shell pwd) -o $@ $<
ifneq "$(CODESIGN)" ""
	$(CODESIGN) -s - "$@"
endif

lib%.dylib: %.swift
	$(MAKE) MAKE_DSYM=YES CC=$(CC) SWIFTC=$(SWIFTC) \
		ARCH=$(ARCH) DSYMUTIL=$(DSYMUTIL) \
		BASENAME=$(shell basename $< .swift) \
		VPATH=$(SRCDIR) -I $(SRCDIR) -f $(SRCDIR)/dylib.mk all

clean::
	rm -rf *.swiftmodule *.swiftdoc *.dSYM *~ lib*.dylib a.out *.o
