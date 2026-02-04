# CMakeLists.txt for the ZoneMinder perl module.

# If this is an out-of-source build, copy the files we need to the binary directory
if(NOT (CMAKE_BINARY_DIR STREQUAL CMAKE_SOURCE_DIR))
  file(COPY "${CMAKE_CURRENT_SOURCE_DIR}/Changes" DESTINATION "${CMAKE_CURRENT_BINARY_DIR}")
  file(COPY "${CMAKE_CURRENT_SOURCE_DIR}/Makefile.PL" DESTINATION "${CMAKE_CURRENT_BINARY_DIR}")
  file(COPY "${CMAKE_CURRENT_SOURCE_DIR}/MANIFEST" DESTINATION "${CMAKE_CURRENT_BINARY_DIR}")
  file(COPY "${CMAKE_CURRENT_SOURCE_DIR}/META.yml" DESTINATION "${CMAKE_CURRENT_BINARY_DIR}")
  file(COPY "${CMAKE_CURRENT_SOURCE_DIR}/README" DESTINATION "${CMAKE_CURRENT_BINARY_DIR}")
  file(COPY "${CMAKE_CURRENT_SOURCE_DIR}/t" DESTINATION "${CMAKE_CURRENT_BINARY_DIR}")
  file(COPY "${CMAKE_CURRENT_SOURCE_DIR}/lib" DESTINATION "${CMAKE_CURRENT_BINARY_DIR}" PATTERN "*.in" EXCLUDE)
endif()

# Create files from the .in files
configure_file(lib/ZoneMinder/Base.pm.in "${CMAKE_CURRENT_BINARY_DIR}/lib/ZoneMinder/Base.pm" @ONLY)
configure_file(lib/ZoneMinder/Config.pm.in "${CMAKE_CURRENT_BINARY_DIR}/lib/ZoneMinder/Config.pm" @ONLY)
configure_file(lib/ZoneMinder/Memory.pm.in "${CMAKE_CURRENT_BINARY_DIR}/lib/ZoneMinder/Memory.pm" @ONLY)
configure_file(lib/ZoneMinder/ConfigData.pm.in "${CMAKE_CURRENT_BINARY_DIR}/lib/ZoneMinder/ConfigData.pm" @ONLY)
configure_file(lib/ZoneMinder/ONVIF.pm.in "${CMAKE_CURRENT_BINARY_DIR}/lib/ZoneMinder/ONVIF.pm" @ONLY)

if(CMAKE_VERBOSE_MAKEFILE)
  set(MAKEMAKER_NOECHO_COMMAND "")
else()
  set(MAKEMAKER_NOECHO_COMMAND "NOECHO=\"1>/dev/null\"")
endif()

# Add build target for the perl modules
add_custom_target(zmperlmodules ALL /home/runner/work/mosdns/mosdns/sdk/staging_dir/hostpkg/usr/bin/perl5.42.0 Makefile.PL AR=ar CC=aarch64-openwrt-linux-gcc CCFLAGS="-D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64 -Os -pipe -mcpu=generic -fno-caller-saves -fno-plt -fhonour-copts -ffile-prefix-map=/home/runner/work/mosdns/mosdns/sdk/build_dir/target-aarch64_generic_musl/zoneminder-1.38.0=zoneminder-1.38.0 -Wformat -Werror=format-security -fstack-protector -D_FORTIFY_SOURCE=1 -Wl,-z,now -Wl,-z,relro -I/home/runner/work/mosdns/mosdns/sdk/staging_dir/toolchain-aarch64_generic_gcc-13.3.0_musl/usr/include -I/home/runner/work/mosdns/mosdns/sdk/staging_dir/toolchain-aarch64_generic_gcc-13.3.0_musl/include -I/home/runner/work/mosdns/mosdns/sdk/staging_dir/toolchain-aarch64_generic_gcc-13.3.0_musl/include/fortify -D_REENTRANT -D_GNU_SOURCE -D_LARGEFILE64_SOURCE" CCCDLFLAGS=-fPIC CCDLFLAGS=-Wl,-E DLEXT=so DLSRC=dl_dlopen.xs EXE_EXT=" " FULL_AR=aarch64-openwrt-linux-ar LD=aarch64-openwrt-linux-gcc LDDLFLAGS="-shared -rdynamic -L/home/runner/work/mosdns/mosdns/sdk/staging_dir/toolchain-aarch64_generic_gcc-13.3.0_musl/usr/lib -L/home/runner/work/mosdns/mosdns/sdk/staging_dir/toolchain-aarch64_generic_gcc-13.3.0_musl/lib -fuse-ld=bfd -znow -zrelro" LDFLAGS="  " LIBC=" " LIB_EXT=.a OBJ_EXT=.o OSNAME=linux OSVERS=2.4.30 RANLIB=: SITELIBEXP=" " SITEARCHEXP=" " SO=so VENDORARCHEXP=" " VENDORLIBEXP=" " SITEPREFIX=/usr INSTALLPRIVLIB=/usr/lib/perl5/5.42 INSTALLSITELIB=/usr/lib/perl5/5.42 INSTALLVENDORLIB=" " INSTALLARCHLIB=/usr/lib/perl5/5.42 INSTALLSITEARCH=/usr/lib/perl5/5.42 INSTALLVENDORARCH=" " INSTALLBIN=/usr/bin INSTALLSITEBIN=/usr/bin INSTALLVENDORBIN=" " INSTALLSCRIPT=/usr/bin INSTALLSITESCRIPT=/usr/bin INSTALLVENDORSCRIPT=" " INSTALLMAN1DIR=/usr/man/man1 INSTALLSITEMAN1DIR=/usr/man/man1 INSTALLVENDORMAN1DIR=" " INSTALLMAN3DIR=/usr/man/man3 INSTALLSITEMAN3DIR=/usr/man/man3 INSTALLVENDORMAN3DIR=" " LINKTYPE=dynamic PERL_INC=/home/runner/work/mosdns/mosdns/sdk/staging_dir/target-aarch64_generic_musl/usr/lib/perl5/5.42/CORE/ FIRST_MAKEFILE=MakefilePerl DESTDIR="${CMAKE_CURRENT_BINARY_DIR}/output" ${MAKEMAKER_NOECHO_COMMAND} COMMAND make -f MakefilePerl pure_install COMMENT "Building ZoneMinder perl modules")

# Add install target for the perl modules
install(DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/output/" DESTINATION "/")

# Add additional files and directories to make clean
set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES "output;blib;pm_to_blib;MakefilePerl")
