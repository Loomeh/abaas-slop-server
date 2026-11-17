# abaas-slop-server

A reimplementation of SEGA's ABaaS service used for networking APM3 games


>[!WARNING]
> The client isn't compatible with TeknoParrot as it injects too much of its own code into the networking stack. If you're not running on a real APM3 machine you must use JConfig/APM3Config

>[!NOTE]
>Currently keeping this closed-source as Sega lawyers scawwy :(


## Compatibility

Theoretically this should work with every APM3 game but these are the ones I've personally tested:

- Guilty Gear Strive

- Under Night In-Birth Exe:Late \[cl-r]



## Client



### Client Configuration

Read [CLIENT_CONFIG.md](https://github.com/Loomeh/abaas-slop-server/blob/main/CLIENT_CONFIG.md)





### Client Setup
1. Download client binaries from Releases

2. Locate your game exe's folder

3. Copy `xinput1_3.dll` (`dsound.dll` if your game is 32-bit) and `abaas.ini` into the folder



## Server



### Server Configuration

Read [SERVER_CONFIG.md](https://github.com/Loomeh/abaas-slop-server/blob/main/SERVER_CONFIG.md)



### Server Setup
_Server was built for Ubuntu Server 24.04 LTS and assumes that you're using the same._

Download server binary from Releases  


2a. Run (Standalone)

```

sudo ./abaas-server -c abaas-server.conf public_host=<IPV4 ADDRESS>

```



2b. Run (systemd service)

```

sudo install -m755 abaas-server /usr/local/bin

sudo install -d /etc/abaas && sudo install -m644 abaas-server.conf /etc/abaas

sudo cp deploy/abaas-server.service /etc/systemd/system/

sudo systemctl daemon-reload && sudo systemctl enable --now abaas-server

```

