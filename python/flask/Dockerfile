FROM python:3.13-slim AS builder

WORKDIR /workspace

#Criptography module´s dependencies 
RUN apt update &&  apt-get -y install rustc cargo

# Copy the project and install Python dependencies from requirements.txt if it exists
COPY requirements.txt .
RUN pip install --user -r requirements.txt


FROM alpine:3.20 AS runner


# Install python
RUN apk add python3

#Create workdir and copy all the code
WORKDIR /workspace
COPY --chown=apiuser app.py .

# Copy the Python dependencies from the builder image and set the PATH
COPY --chown=apiuser --from=builder /root/.local /home/apiuser/.local
ENV PATH=/home/apiuser/.local/bin:$PATH

# Create a user to run the application with bash shell and home directory
RUN adduser -D apiuser 
USER apiuser

# Run the flask application
EXPOSE 80
ENTRYPOINT ["python3","app.py"]

