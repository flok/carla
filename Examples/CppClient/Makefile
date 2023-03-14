DIR := ${CURDIR}

CARLADIR=$(DIR)\..\..
BUILDDIR=$(DIR)\build
BINDIR=$(DIR)\bin
INSTALLDIR=$(DIR)\libcarla-install

CXXFLAGS=-std=c++14 -pthread -fPIC -O3 -DNDEBUG -Werror -Wall -Wextra

define log
	@echo "\033[1;35m$(1)\033[0m"
endef

default: build

run: build
	$(call log,Running C++ Client...)
	@$(BINDIR)\cpp_client $(ARGS)

run.only:
	$(call log,Running C++ Client...)
	@$(BINDIR)\cpp_client $(ARGS)

build: $(BINDIR)\cpp_client

$(BINDIR)\cpp_client: | build_libcarla
	$(call log,Compiling C++ Client...)
	@mkdir "$(BINDIR)"
	@cl.exe $(CXXFLAGS) -I$(INSTALLDIR)\include -isystem $(INSTALLDIR)\include\system -L$(INSTALLDIR)\lib \
		-o $(BINDIR)/cpp_client main.cpp \
		-Wl,-Bstatic -lcarla_client -lrpc -lboost_filesystem -Wl,-Bdynamic \
		-lpng -ltiff -ljpeg -lRecast -lDetour -lDetourCrowd

build_libcarla: $(TOOLCHAIN)
	@cd $(CARLADIR) && make setup
	@mkdir "$(BUILDDIR)"
	$(call log,Compiling LibCarla.client...)
	@cd $(BUILDDIR) && \
		cmake -DCMAKE_BUILD_TYPE=Client -DLIBCARLA_BUILD_RELEASE=ON -DLIBCARLA_BUILD_DEBUG=OFF -DLIBCARLA_BUILD_TEST=OFF -DCMAKE_INSTALL_PREFIX=$(INSTALLDIR) -DCMAKE_EXPORT_COMPILE_COMMANDS=1 $(INSTALLDIR) $(CARLADIR) && \
		msbuild CARLA.sln /property:Configuration=Release -maxCpuCount:100 /m:100


