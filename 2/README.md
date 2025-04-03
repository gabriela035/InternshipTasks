# FOR notes.js APPLICATION
Open both notes.js and package.json in VSCode (or any IDE), install dependencies (`npm install`) and then run the application with `npm start` (install node and express if necessary)
As requested, change port to 8080.

![image](https://github.com/user-attachments/assets/78197637-6f02-460d-89ef-a8b17b6a2118)

Following *docs.docker*, in app folder, add a new file named Dockerfile with:

`FROM node:16`  <sup>Use an official Node.js runtime as a parent image</sup> 

`WORKDIR /app` <sup>Set the working directory in the container</sup>

`COPY package*.json ./` <sup>Copy package.json and package-lock.json</sup>

`RUN npm install` <sup>Install dependencies</sup>

`COPY . .` <sup>Copy the rest of the application files</sup>

`EXPOSE 8080` <sup>Expose the port the app will run on</sup>

`CMD ["node", "notes.js"]` <sup>Define the command to run the app</sup>


## Build the Docker image locally
Open terminal and run docker `build -t app`. After some time, it should be like this:

![image](https://github.com/user-attachments/assets/f4c59c9f-4482-4f83-93ad-81fc4282bd8b)

You can see the built local image in Docker Desktop

![image](https://github.com/user-attachments/assets/4619f815-4a69-4d3c-8d82-f3dc2c3cc96f)



## Start an app container
In terminal run `docker run -d -p 127.0.0.1:8080:8080 app`
so the container is created in Docker Desktop and its running 🏃. It can be visible in docker logs.

![image](https://github.com/user-attachments/assets/ad44fb53-a17e-40e9-80ce-f64d343b039b)![image](https://github.com/user-attachments/assets/d514f0b1-fef5-4d21-a91a-c9ba048a3838)





To test if it works, open the port from Docker and insert some data. 

![image](https://github.com/user-attachments/assets/3659dbda-fe39-4551-9775-25f2ab32a4da)



