FROM alpine/git as source

RUN git clone                              \
        --depth=1                          \
        --recurse-submodules               \
	--shallow-submodules               \
        --single-branch                    \
        -b v4.8.6                          \
        https://github.com/qt/qt.git       \
        /qt-src

FROM centos:6 as library

RUN mkdir /qt-src /qt 
COPY --from=source /qt-src /qt-src                                    

RUN yum check-update && yum groupinstall -y 'Development Tools'
RUN yum check-update && yum install -y     \    
    libX11-devel                           \
    libXext-devel                          \
    libXtst-devel                          \
    mesa-libGL-devel

WORKDIR /qt-src

RUN ./configure                             \
        -static                             \
        -release                            \
        -confirm-license                    \
        -platform linux-g++-64              \
        -silent                             \
        -prefix /qt                         \
        -opensource                         \
        -nomake demos                       \
        -nomake examples                    \
        -nomake tests                       \
        -nomake docs                        \
        -nomake translations                \
        -no-fast                            \
        -no-exceptions                      \
        -no-qt3support                      \
        -opengl desktop                     \
        -qt-libpng                          \
        -qt-libmng                          \
        -qt-libtiff                         \
        -qt-libjpeg                      && \
    make && make install

FROM centos:6

RUN mkdir /qt /workspace

COPY --from=library /qt /qt

ENV PATH=/qt/bin:$PATH

WORKDIR /workspace

CMD ["/bin/bash"]
