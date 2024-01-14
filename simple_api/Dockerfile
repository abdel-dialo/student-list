FROM python:2.7-buster
MAINTAINER Abdoul Gadirou DIALLO <diallo.abdoulgadirou@gmail.com>
ADD ./student_age.py" /
RUN apt update -y
RUN  apt install python-dev python3-dev libsasl2-dev python-dev libldap2-dev libssl-dev -y
RUN pip install flask==1.1.2 flask_httpauth==4.1.0 flask_simpleldap python-dotenv==0.14.0
EXPOSE 5000
CMD [ "python", "./student_age.py" ]


