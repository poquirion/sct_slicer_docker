FROM poquirion/compiled_slicer
MAINTAINER Pierre-Olivier Quirion <poq@criugm.qc.ca>

# download slicer

ENV SLICER_DIR /usr/local/Slicer
ENV SCT_DIR /usr/local/sct


#RUN cd /tmp/slicer_build/Slicer-build && make install 

RUN cd /usr/local/bin && ln -s /tmp/slicer_build/Slicer-build/Slicer Slicer

#RUN /usr/local/Slicer
RUN rm -r $SCT_DIR && cd /usr/local \ 
    && git clone https://bitbucket.org/OrthopaedicBiomechanicsLab/spinalcordtoolbox sct \
    &&  cd $SCT_DIR && git checkout issue_1482_slicer

 
#RUN git apply $SCT_DIR/install/Slicer_patch
RUN mkdir -p  /tmp/build_ant && cd /tmp/build_ant && git clone --depth 1 https://github.com/benjamindeleener/ANTs.git 

RUN PATH=/tmp/slicer_build/ITKv4-build/:/tmp/slicer_build/VTKv8-build/:${PATH} \
    && mkdir /tmp/build_ant/build &&  cd /tmp/build_ant/build && cmake ../ANTs

RUN PATH=/tmp/slicer_build/ITKv4-build/:/tmp/slicer_build/VTKv8-build/:${PATH} && cd ${SCT_DIR}&& git pull && cd ${SCT_DIR}/install && ./make_binaries.sh

RUN cd /tmp/build_ant/build  && make -j 7 
