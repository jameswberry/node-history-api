# Install Docker
https://www.docker.com/get-started

# Install Kitematic
https://kitematic.com/download

## Docker Node Container
https://hub.docker.com/_/node/

$ docker pull node

## Docker Redis Container
https://hub.docker.com/_/redis/

$ docker pull redis

## Install Redis Desktop
https://redisdesktop.com/

1 Install XCode with Xcode build tools (https://developer.apple.com/xcode/)
1 Install Homebrew (http://brew.sh/)

### Install Homebrew
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
$ brew install openssl cmake

### Install Redis Desktop Manager
$ git clone --recursive https://github.com/uglide/RedisDesktopManager.git -b 0.9 rdm && cd ./rdm
$ cd ./src && cp ./resources/Info.plist.sample ./resources/Info.plist
$ ./configure

### Install Qt Creator
$ wget http://download.qt.io/official_releases/online_installers/qt-unified-mac-x64-online.dmg

### Add Qt Charts module
### Open ./src/rdm.pro in Qt Creator
### Run build


## Node Package
$ npm install redis

## Redis Config

### To ENABLE Append Only File (AOF) run the following command in the Redis installation folder:
$ redis-cli config set appendonly yes

### To DISABLE Append Only File (AOF) run the following command in the Redis installation folder:
$ redis-cli config set save ""

## Redis hello-world
* To check if the server is running you can either use Redis Desktop and connect to the server (127.0.0.1 for localhost) or in the command line type redis-cli ping (it should respond with ‘PONG’)
* Create a folder for the project and run npm init
* Install redis for node.js with npm install redis --save
* Create an index.js file with the code below and run it with node index.js:
	var redis = require('redis');
	var client = redis.createClient();
	client.on('error', function(err){
		console.log('Something went wrong ', err)
	});
	client.set('my test key', 'my test value', redis.print);
	client.get('my test key', function(error, result) {
		if (error) throw error;
		console.log('GET result ->', result)
	});
	This will create a record in the database which you can access with Redis Desktop.
* Or in the command line:
	$ redis-cli get 'my test key'
