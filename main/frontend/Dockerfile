FROM node

# working dir for app
WORKDIR /app

# to copy .json file into /app
COPY package.json .  

# to install all dependencies
# Used during the build phase to install dependencies. This instruction is for building the image.
# RUN is for building the environment in the image
RUN npm install

# copy all file from local to docker
COPY . .

# to expose contaier to port 80 -> mention in code and cmd
EXPOSE 3000

# the command that will be run when a container is started from the image,
# When it Executes: It runs when a container starts
# CMD is for specifying what to run when the container starts.
CMD [ "npm", "start" ]