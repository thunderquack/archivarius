# Use an official Node runtime as a parent image
FROM node:22 as build

# Set the working directory
WORKDIR /usr/src/app

# Build arguments
ARG VERSION

# Create a temporary directory for cloning the repository
RUN mkdir /tmp/repo

# Clone the specific version of the repository (branch or tag)
RUN git clone --branch $VERSION https://github.com/thunderquack/arc-fe.git /tmp/repo

# Copy the repository content into the working directory and remove the temporary directory
RUN cp -r /tmp/repo/* . && rm -rf /tmp/repo

# Install app dependencies
RUN npm install

# Build the Angular app
RUN npm run build --prod

# Use a lightweight web server to serve the static files
FROM nginx:bookworm

# Copy the built Angular files from the previous stage
COPY --from=build /usr/src/app/dist/arc-fe/browser /usr/share/nginx/html

# Copying nginx config
COPY ./nginx.conf /etc/nginx/conf.d/default.conf

# List files in the nginx html directory to verify the copy
RUN ls -l /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx server
CMD ["nginx", "-g", "daemon off;"]
