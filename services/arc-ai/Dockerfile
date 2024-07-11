FROM python:3.12-slim

# Set the working directory
WORKDIR /app

# Build arguments
ARG VERSION

# Create a temporary directory for cloning the repository
RUN mkdir /tmp/repo

RUN apt-get update && apt-get install -y git && apt-get clean

# Clone the specific version of the repository (branch or tag)
RUN git clone --branch $VERSION https://github.com/thunderquack/arc-ai.git /tmp/repo

# Copy the repository content into the working directory and remove the temporary directory
RUN cp -r /tmp/repo/* . && rm -rf /tmp/repo

# Install app dependencies
RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 8000

CMD ["python", "app.py"]