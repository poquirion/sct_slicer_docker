FROM poquirion/installed_slicer
MAINTAINER Pierre-Olivier Quirion <poq@criugm.qc.ca>

# download slicer

ENV SLICER_DIR /usr/local/Slicer
ENV SCT_DIR /usr/local/sct
ADD Slicer /usr/local/sct/Slicer
RUN rm /usr/local/bin/Slicer && cd /tmp/slicer_build/Slicer-build && make package  

#RUN PATH=/tmp/slicer_build/ITKv4-build/:/tmp/slicer_build/VTKv8-build/:${PATH} && cd ${SCT_DIR}&& git pull && cd ${SCT_DIR}/install && ./make_binaries.sh

#RUN cd /tmp/build_ant/build  && make -j 7 
WORKDIR $SCT_DIR/bin
RUN for name in antsRegistration antsSliceRegularizedRegistration antsApplyTransforms ANTSLandmarksBSplineTransform ComposeMultiTransform ; do cp /tmp/build_ant/build/bin/${name} ${SCT_DIR}/bin/isct_${name}; done

WORKDIR /tmp
#RUN Slicer --launcher-no-splash --no-main-window  --python-script /tmp/pip_install.py -c 'print("DONE")'
RUN cd /usr/local/ && tar -xvf  /tmp/slicer_build/Slicer-build/Slicer-4.7.0-2017-10-11-linux-amd64.tar.gz 
ADD slicer_env /tmp/slicer_env
RUN rm /bin/sh && cd /bin && ln -s bash sh

RUN cd /tmp/ && curl -L https://downloads.sourceforge.net/project/pyqt/PyQt4/PyQt-4.12.1/PyQt4_gpl_x11-4.12.1.tar.gz> pyqt.tgz && tar -zxvf pyqt.tgz 
RUN cd /tmp/ && curl -L https://downloads.sourceforge.net/project/pyqt/sip/sip-4.19.3/sip-4.19.3.tar.gz > sip.tgz && tar -zxvf sip.tgz
RUN cd /tmp/sip-4.19.3 && eval $(cat /tmp/slicer_env) && yes | /usr/local/Slicer-4.7.0-2017-10-11-linux-amd64/bin/python-real configure.py -k  CFLAGS+=-I/tmp/slicer_build/Python-2.7.13/Include && make -j 7  && make install
RUN cd /tmp/PyQt4_gpl_x11-4.12.1 && eval $(cat /tmp/slicer_env) && /usr/local/Slicer-4.7.0-2017-10-11-linux-amd64/bin/python-real configure-ng.py --sip /usr/local/Slicer-4.7.0-2017-10-11-linux-amd64/lib/Python/bin/sip --confirm-license  #&& make -j 7 && make install

RUN cp -r /tmp/slicer_build/python-install/include/python2.7/* /usr/local/Slicer-4.7.0-2017-10-11-linux-amd64/lib/Python/include/python2.7/.
RUN cd ${SCT_DIR} && echo hop && git pull
ADD pip_install.py /tmp/pip_install.py
RUN eval $(cat /tmp/slicer_env) &&  /usr/local/Slicer-4.7.0-2017-10-11-linux-amd64/bin/python-real pip_install.py
