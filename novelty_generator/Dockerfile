FROM      maven:3

MAINTAINER Mohamed Boussaa <mohamed.boussaa@inria.fr>

#ENV MAVEN_VERSION 3.3.3

RUN apt-get upgrade
RUN apt-get update -y

#RUN apt-get install openssh-client curl -y

RUN apt-get install -y git

#RUN curl -fsSL http://archive.apache.org/dist/maven/maven-3/   
#$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz | #tar xzf - -C /usr/share \
#  && mv /usr/share/apache-maven-$MAVEN_VERSION /usr/share/maven \
#  && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn
#ENV MAVEN_HOME /usr/share/maven

#RUN git clone https://github.com/mboussaa/script-generator-for-ffmpeg.git

#COPY /home/mboussaa/script-generator-for-ffmpeg /root/
#WORKDIR /script-generator-for-ffmpeg
#RUN mvn clean
#RUN mvn install
#RUN cp -r /script-generator-for-ffmpeg/ffmpeg/* /tmp/
#WORKDIR /script-generator-for-ffmpeg/target/ffmpeg
#RUN ls -l
#RUN mkdir /sourcecode
#RUN mv /script-generator-for-ffmpeg/ffmpeg /tmp
#WORKDIR /tmp/ffmpeg
#RUN ls -l

#CMD ["mvn"]
