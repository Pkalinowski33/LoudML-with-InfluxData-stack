# LoudML + InfluxData
#### This is a quick guide how to install InfluxData products including LoudML features.
***
## Requirements:
    Ubuntu 18.04.02 LTS AMD64
***
# Overview
##### Stack includes:
    Telegraf
    InfluxDB
    Chronograf for Loud ML
    Kapacitor
    Loud ML Community Edition

##### The following ports are exposed on the host:

    8077: Loud ML
    8086: InfluxDB
    9092: Kapacitor
    8888: Chronograf
    8080: Chronograf with LoudML extension
***
# Installation
#### 1.The first step is to download and extract InfluxData products from repo:
Telegraf:

```
wget https://dl.influxdata.com/telegraf/releases/telegraf_1.10.2-1_amd64.deb
sudo dpkg -i telegraf_1.10.2-1_amd64.deb
```
InfluxDB:
```
wget https://dl.influxdata.com/influxdb/releases/influxdb_1.7.5_amd64.deb
sudo dpkg -i influxdb_1.7.5_amd64.deb
```

Chronograf:
```
wget https://dl.influxdata.com/chronograf/releases/chronograf_1.7.9_amd64.deb
sudo dpkg -i chronograf_1.7.9_amd64.deb
```
Kapacitor:
```
wget https://dl.influxdata.com/kapacitor/releases/kapacitor_1.5.2_amd64.deb
sudo dpkg -i kapacitor_1.5.2_amd64.deb
```
---
#### 2. Next step is to install LoudML
Download and install the public signing key:

`wget -qO - http://loudml.s3-website-eu-west-1.amazonaws.com/repo/gpg/GPG-KEY-LOUDML | sudo apt-key add -`

Install Dependencies:

`apt-get install python3-lib2to3`

`sudo apt-get install apt-transport-https`

Save the repository definition in /etc/apt/sources.list.d/loudml-1.x.list:

`echo "deb http://loudml.s3-website-eu-west-1.amazonaws.com/repo/1.x/ubuntu/18.04/amd64 bionic main" | sudo tee -a /etc/apt/sources.list.d/loudml-1.x.list`

And then:

`sudo apt-get update && sudo apt-get install loudml`

---
#### 3.Install Go
`wget https://dl.google.com/go/go1.12.2.linux-amd64.tar.gz`

Extract:

`tar -C /usr/local -xzf go1.12.2.linux-amd64.tar.gz`

Add /usr/local/go/bin to the PATH environment variable. You can do this by adding this line to your /etc/profile (for a system-wide installation) or $HOME/.profile:

`export PATH=$PATH:/usr/local/go/bin`

Test your installation:

Create your workspace directory, %USERPROFILE%\go

`export GOPATH=$HOME/go ---> (go env GOPATH)`

Next, make the directory src/hello inside your workspace, and in that directory     create a file named hello.go that looks like: 
```
package main

import "fmt"

func main() {
	fmt.Printf("hello, world\n")
}
```

Then build it with the go tool:

`cd $USERPROFILE/go/src/hello`
    
`go build`
    
execute to see message:
`./hello`

    hello, world
---
#### 4. Install Node and NPM
Download Node From Source:

`wget https://nodejs.org/dist/v10.15.3/node-v10.15.3.tar.gz`

Download and install Prerequisites:

`sudo apt-get install gcc clang python make`

Extract Node:

`tar -xvzf node-v10.15.3.tar.gz`

Go to extracted directory and build node:

`cd $nodedir`

`./configure`

`make j4`

Test Node and npm:

`Node -v`

`npm -v`

---
#### 5. Install Yarn
prerequisites:

`sudo apt-get install curl`

add key:

`curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -`

save the repository:

`echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list`

and type simply:

`sudo apt-get update && sudo apt-get install yarn`

test your installation:

`yarn --version`

#### 6. Run all your installed services:
InfluxDB: `sudo systemctl start influxd`

Kapacitor: `sudo systemctl start kapacitor`

Telegraf: `sudo systemctl start telegraf`

Chronograf: `sudo systemctl start chronograf`

---
#### 7.Get Chronograf Sources:

`go get github.com/influxdata/chronograf`

Setup $HOME/go/src/github.com/regel/chronograf as remote:

`cd src/github.com/influxdata/chronograf`
`git remote add regel git@github.com:regel/chronograf.git`

Switch to regel/loudml branch:

`git fetch regel`
`git checkout -b loudml regel/loudml`

and then build chronograf with loudML extension:

`make`

To start chronograf with loudml type:

`cd chronograf/ui`

`npm install`

`yarn dev`

---
#### 8. Open your browser, type: localhost:8080 and see that LoudML extension is viable in chronograf.
***
# References
    https://github.com/regel/chronograf
    https://yarnpkg.com/en/docs/install#debian-stable
    https://golang.org/doc/install
    https://loudml.io/guide/en/loudml/reference/current/ubuntu.html
    https://github.com/regel/loudml/tree/master/docker/compose
