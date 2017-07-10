FROM ubuntu:16.04
MAINTAINER Blake Dewey

# Build/Software Directories
ENV BUILDDIR /opt/builds
ENV SOFTDIR /opt/software

# Install Variables
ENV N_CPU 2
ENV ANTS_VERSION 2.2.0
ENV ANTS_COMMIT_HASH 2.2.0
ENV ANTS_GIT https://github.com/stnava/ANTs.git

# Install ANTs
RUN \
    apt-get update && \
    apt-get -y upgrade && \
    apt-get -y install gcc g++ cmake git zlib1g-dev && \
    mkdir -p ${SOFTDIR} ${BUILDDIR} && \
	cd ${BUILDDIR} && \
    git clone ${ANTS_GIT} ants && \
    cd ants && \
    if [ ${ANTS_VERSION} == 'dev' ]; then git checkout $ANTS_COMMIT_HASH; else git checkout tags/v$ANTS_VERSION; fi && \
    cd ${BUILDDIR} && \
    mkdir ants-build && \
    cd ants-build && \
    cmake ../ants -DCMAKE_INSTALL_PREFIX=${SOFTDIR}/ants \
        -DCMAKE_BUILD_TYPE=Release \
        -DBUILD_SHARED_LIBS=ON && \
    make -j ${N_CPU} && \
    mkdir ${SOFTDIR}/ants && \
    mv bin lib ${SOFTDIR}/ants/ && \
    mv ITKv4-install ${SOFTDIR}/ants/ITKv4 && \
    cd ${BUILDDIR} && \
    mv ants/Scripts ${SOFTDIR}/ants/scripts && \
    rm -rf ants* && \
    apt-get -y remove cmake git gcc g++ zlib1g-dev && \
    apt-get -y autoremove
	
ENV PATH ${SOFTDIR}/ants/bin:${SOFTDIR}/ants/scripts:${PATH}
ENV LD_LIBRARY_PATH ${SOFTDIR}/ants/lib:${SOFTDIR}/ants/ITKv4/lib:${LD_LIBRARY_PATH}
ENV LIBRARY_PATH ${SOFTDIR}/ants/lib:${SOFTDIR}/ants/ITKv4/lib:${LIBRARY_PATH}
ENV CPATH ${SOFTDIR}/ants/ITKv4/include:${CPATH}
ENV ANTSPATH ${SOFTDIR}/ants/bin
ENV ITK_DIR ${SOFTDIR}/ants/ITKv4

CMD ["/bin/bash"]