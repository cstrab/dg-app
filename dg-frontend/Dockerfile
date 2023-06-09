# Use base node image for the build stage
FROM node:18.16.0 as build

# Set the working directory in the container to /app
WORKDIR /app

# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

# Install the application dependencies
RUN npm install

# Copy the rest of the application code to the working directory
COPY . .

# Create a production build of the app
RUN npm run build

# Start the serve stage
FROM nginx:1.21.1

# Remove the default nginx.conf
RUN rm /etc/nginx/conf.d/default.conf

# Copy the custom default.conf to nginx
COPY default.conf /etc/nginx/conf.d/

# Copy the built files from the build stage to the nginx html directory
COPY --from=build /app/build /usr/share/nginx/html

# Add a new entrypoint file
COPY ./entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Expose port 3000 for the application
EXPOSE 3000

# Make the entrypoint file the default command for the container
CMD ["/entrypoint.sh"]
