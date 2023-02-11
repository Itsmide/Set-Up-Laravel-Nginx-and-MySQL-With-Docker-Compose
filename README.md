Set Up Laravel, Nginx, and MySQL With Docker Compose

             Prerequisite
- Get a VM machine (Preferrably DigitalOcean)
- Make sure Docker is Installed.
- For the sake of the project, I created a new user and Its own directory inside the HOME folder( Give it a password you won't forget ).

. Make sure git is installed
- $ sudo apt install git

. Add user to sudo Group 
- $ usermod -aG sudo newuser

. Switch to new user
- $ su newuser

. Executing docker command without sudo ( To avoid typing sudo whenever you run docker command)
- $ sudo usermod -aG docker ${USER}

. To allow new membership, log out of the server and log back in
- $ su - ${USER}
  * You will be prompted to enter your user’s password to continue.

. Confirm that your user is now added to the docker group by typing:
- $ groups

. If you need to add a user to the docker group that you’re not logged in as
- $ sudo usermod -aG docker username

. Install Docker-Compose and set correct permisssions so that docker-compose command is executable
- $ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
- $ sudo chmod +x /usr/local/bin/docker-compose

. Verify installation with:
- $ docker-compose --version

. Now, go to the root (Don't forget the root is the /home/Itsmide, since you are operating as a user) 
- $ cd ~

. Clone The Laravel Repo
- $ git clone https://github.com/laravel/laravel.git laravel-app

. Move into the laravel-app directory
- $ cd ~/laravel-app

. Set permissions on the project directory so that it is owned by your non-root user
- $ sudo chown -R newuser:newuser ~/laravel-app

. In the docker-compose.yml file, make sure the user is changed to your new user as well as image name.
. Replace the environment variable MYSQL ROOT PASSWORD under the db service with a secure password of your choice.
. Then, save code.

. Copy file from .env.example to .env in the laravel Directory
- $ cp .env.example .env

. Open file .env (nano .env ) and Change DB configurations from default to yours:
- Example 
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=Itsmide
DB_PASSWORD=your_laravel_db_password  

. Then, save file.

. Use command to build the app image 
- $ docker-compose build app

. When it is done type in the terminal to run the environment in the background,
- $ docker-compose up -d

. To show information about the state of your active services, run:
- $ docker-compose ps

. Now, check if you see anything like composer.lock in the outputs. if there isn’t any then you can move on with the next process but in this case, there is so we run this command to remove it:
- $ docker-compose exec app rm -rf vendor composer.lock

. Run composer install to install application dependencies:
- $ docker-compose exec app composer install

. Before testing the application, the last step is to create a special application key using the artisan Laravel command-line tool.
- $ docker-compose exec app php artisan key:generate

. Check the IP Address and you don’t need to add any port to it because we already specified a port 80 in the docker-compose.yml file.
