<p align="center">
    <img src="resources/overledger-network.png">
</p>
<p align="center">
    <a href="https://github.com/quantnetwork/overledger-network-gateway/releases" alt="Releases">
        <img src="https://img.shields.io/github/v/release/quantnetwork/overledger-network-gateway?include_prereleases" /></a>
    <a href="https://twitter.com/intent/follow?screen_name=quant_network">
        <img src="https://img.shields.io/twitter/follow/quant_network?style=social"
            alt="follow on Twitter"></a>
</p>

The instructions below outline the steps to run an Overledger Network Gateway.

## Requirements
- Docker
- Public IP
- Ports 8080 and 11337 open
- BPI Key

## Getting Started
First, run a local instance of MongoDB through Docker:
```sh
docker run -d --name quant-mongo -p 27017:27017 mongo:latest
```

## Running
We will now run a container based on the Docker image for the Overledger Network Gateway, passing in the relevant environment variables.

Make sure to replace the values for GATEWAY_ID with your BPI Key, and for the GATEWAY_HOST and MONGO_DB_HOST with the Public IP of your machine.
>Note: on OS X, you will have to replace the value for MONGO_DB_HOST with "host.docker.internal"
```sh
docker run -dit \
    --name overledger-network-gateway \
    -p 8080:8080 -p 11337:11337 \
    -e GATEWAY_ID="bpiKey" \
    -e GATEWAY_HOST="127.0.0.1" \
    -e MONGO_DB_HOST="127.0.0.1" \
    quantnetwork/overledger-network-gateway:latest
```


After running the container, we can follow the logs using:
```sh
docker logs -f overledger-network-gateway
```

The Gateway will start up with a random number of connectors between C1 and C10.
Make sure when submitting requests that you submit them for the connectors that are up, as they toggle on and off on a scheduled basis. You can find the active connectors by checking the latest log entries.

## API

We can now submit mock requests to our Gateway, using the /do-task API:
POST http://localhost:8080/tasks
with the body:
```json
{
	"connectorId": "C1",
	"task": "Send transaction."
}
```