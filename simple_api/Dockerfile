ARG VERSION="3.8-buster"
FROM python:$VERSION
MAINTAINER Abdoul Gadirou DIALLO <diallo.abdoulgadirou@gmail.com>
ADD ./student_age.py /
ADD ./requirements.txt /
RUN apt update -y
RUN  apt install python-dev python3-dev libsasl2-dev python-dev libldap2-dev libssl-dev -y
RUN pip3 install -r ./requirements.txt 
EXPOSE 5000
CMD [ "python", "./student_age.py" ]


