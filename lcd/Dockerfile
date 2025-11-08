FROM anapsix/alpine-java:8_jdk_nashorn
RUN mkdir /myapp
COPY lcd-0.0.1-SNAPSHOT.jar /myapp/lcd.jar
CMD ["java", "-jar", "/myapp/lcd.jar"]
