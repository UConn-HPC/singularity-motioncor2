# -*- mode: rpm-spec -*-

# Released into the Public Domain:
# https://creativecommons.org/publicdomain/zero/1.0/legalcode

Bootstrap: docker
From: nvidia/cuda:8.0-runtime-centos7

%help
MotionCor2 version 1.2.6 for CUDA 8.0

%files
# Copy the pre-built binary from the current directory into the
# container.
MotionCor2_1.2.6-Cuda80 /usr/bin

%post
exe=MotionCor2_1.2.6-Cuda80
prefix=/usr/bin
# Install libtiff 4
yum -y install libtiff
# MotionCor2 comes as a pre-built binary which needs to link against
# CUDA and LIBTIFF.  Check the output of `ldd` to confirm all
# dependencies are satisfied.
#
# Fail if any OS packages are missing, and suggest packages to install.
echo "Searching for missing libs..."
missing_libs=$( ldd $prefix/$exe | grep 'not found' | awk '{print $1}' )
# Ignore libcuda.so.1 which is not available except at runtime.
missing_libs=${missing_libs/libcuda.so.1/}
if [ -n "${missing_libs}" ]; then
    echo >&2 "
Error: the following libraries are not yet installed:
$missing_libs

Suggesting packages to install...
"
    for lib in $missing_libs; do
        yum -q whatprovides *lib*/$lib
    done
    echo >&2 "
The above package set likely contains packages that depend on other packages.
To install the minimal required package set, check the output of each package
using repoquery.  Namely:

   repoquery --requires PACKAGE_NAME
"
    exit 1
fi

%runscript
exec MotionCor2_1.2.6-Cuda80 "$@"
