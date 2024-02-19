# YCOus Backend

This repository contains the backend code for the YCOus website. It includes routes for managing the captcha server and handling the GET/POST requests of the YCOus frontend website. The main file, `main.js`, is the entry point for the backend server.

## Prerequisites

Make sure you have the following installed on your machine:

- Linux Server enviroment (preferably using AWS EC2)
- Node.js
- npm (Node Package Manager)
- MongoDB (for database connectivity)
- .env file with necessary variables (See "Configuration" section)
- Docker
- Domain name( for generating SSL certificates using Certbot)

## Installation

1. Clone the repository:

   ```bash
   git clone <repository-url>

2. Install dependencies:

   ```bash
   npm install

## Configuration

Create a .env file in the root directory of the project using the generate_env.sh file.

```bash
# Specify the filename (.env) where you want to save the content
output_file=".env"

# Use a here document to write a formatted string with line breaks

cat <<EOF > "$output_file"

CAPTCHA_TEXT_LENGTH=          # Length of Captcha
CAPTCHA_LIFETIME =            # In Minutes
CRON_EXPRESSION =" "           # Use your cron expression here
MONGOOSE_URL=" "               # Use your mongoose databaseurl
HTTPS_PORT=                    # Use your desired port to run application on
EMAIL_ID=                      # Email to be used
PASS=                          # Password generated with Google. Google account -> app passwords

EOF

echo "Content has been written to $output_file."
```

Generate the SSL certificates using the following command :

 ```bash
 sudo certbot certonly --standalone -d yourdomain.com
```
Locate the certificates generated and move them to the Keys Folder.

## Routes

- **GET /**
  - Redirects to the YCOus frontend index page.

- **POST /send**
  - Handles the form submission for sending emails. Requires the following parameters:
    - firstname
    - lastname
    - phonenumber
    - email
    - message

- **POST /get-captcha**
  - Generates a captcha image and provides the necessary details. No parameters required.

- **POST /is-human**
  - Verifies if the provided captcha answer is correct. Requires the following parameters:
    - uuid
    - answer

- **POST /is-verified**
  - Checks if the provided captcha UUID is verified. Requires the following parameter:
    - uuid

## Usage

1. **On Local machine:**
   
```bash
docker-compose up --build
```
Once the Docker containers are up and running, you can access the backend services through localhost:<https_port> .

2. **On AWS EC2 server:**
   
   1. Login to docker using credentials on the EC2 web server as well as the local environment .
   2. Push the docker image from local machine
      ```bash
     docker push IMAGE_NAME :[TAG]
     ```
   3. On the EC2 server pull the image
      ```bash
     docker pull IMAGE_NAME :[TAG]
     ```
   4. Copy your compose.yml and .env file to a Folder and run the following commands inside the same folder:
      ```bash
      docker-compose build
      docker-compose up -d
      ```
Once the container is running , you can access it using your server IP address or the DNS of the server followed by ":<https_port>".
     
     
     
   
