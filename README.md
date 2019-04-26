# Ganbreeder fork

Setup a PostgreSQL database on MacOS: https://www.robinwieruch.de/postgres-sql-macos-setup/

Changes to the GAN server launch described below, when using MacOS Mojave:

- Tensorflow version, change to 1.13.1 because 1.12.0 isn't available.
- To install dependencies: `pip3 install -r requirements.txt`
- To start server: `python3 server.py`

Changes to frontend server launch:

- Create the directory `server/public/img` before running `node make_randoms.js`.

Original Ganbreeder Readme from here.

# Ganbreeder

[Ganbreeder](https://ganbreeder.app) is a collaborative art tool for discovering images. Images are 'bred' by having children, mixing with other images and being shared via their URL. This is an experiment in using breeding + sharing as methods of exploring high complexity spaces. GAN's are simply the engine enabling this. Ganbreeder is very similar to, and named after, Picbreeder. It is also inspired by an earlier project of mine [Facebook Graffiti](http://www.joelsimon.net/facebook-graffiti.html) which demonstrated the creative capacity of crowds.Ganbreeder uses [these](https://tfhub.dev/deepmind/biggan-128/2) models.

This code was made in a weekend and hasn't been cleaned up or documented yet. There are also improvements to make to scalability.

Pull request are more than welcome :)

## How to use

### Prerequisites
* Install Python 3 + pip (for the GAN server)
* Install NodeJS + npm (for the frontend)
* Install a PostgreSQL server

### Launch the GAN server
```bash
cd gan_server
# Install dependencies
pip install -r requirements.txt
# And go...
python server.py
```
Your GAN server is available at http://localhost:5000/

### Configure the frontend
For quick hacking, if you have Docker at your disposal, you can spawn a PostgreSQL database like so:
```bash
docker run -p 5432:5432 --name ganbreederpostgres -e POSTGRES_PASSWORD=ganbreederpostgres -d postgres
```
With that simple scenario, the database and user would be `postgres` and the password would be `ganbreederpostgres`

Copy the file `server/example_secrets.js` to `secrets.js` and modify it to fit your environment.

### Launch the frontend
```bash
cd server
npm install
# Create the database structure
node_modules/knex/bin/cli.js migrate:latest
# Generate the first images
node make_randoms.js
# Generate a cache of image keys for the front page (do it every time you want to update the front page)
node updatecache.js
# And go...
node server.js
```
Your frontend is available at http://localhost:8888/
