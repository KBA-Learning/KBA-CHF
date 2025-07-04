# Automobile Application
 
 <b>Note:</b> Clone this repository & copy <b>fabric-samples</b> folder to the cloned repository named <b>KBA-CHF </b>.
       Execute <b>go mod tidy</b> command within the Chaincode, Client and Events folder.

# Commands For Executing this application
## Bring up the Test network

```
cd fabric-samples/test-network
```
```
./network.sh up createChannel -c autochannel -ca -s couchdb
```
## Addding org3
```
cd addOrg3
```
```
./addOrg3.sh up -c autochannel -ca -s couchdb
```
```
cd ..
```
## Checking docker containers
```
docker ps -a
```
## Deploy chaincode
```
./network.sh deployCC -ccn KBA-Automobile -ccp ../../KBA-Automobile/Chaincode/ -ccl go -c autochannel -cccg ../../KBA-Automobile/Chaincode/collections.json

```

## General Environment variables:

```
export FABRIC_CFG_PATH=$PWD/../config/

export ORDERER_CA=${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem


export ORG1_PEER_TLSROOTCERT=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt


export ORG2_PEER_TLSROOTCERT=${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt


export CORE_PEER_TLS_ENABLED=true

```
## Environment variables for Org1:
```
export CORE_PEER_LOCALMSPID=Org1MSP

export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt

export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp

export CORE_PEER_ADDRESS=localhost:7051
```
## Invoke – CreateCar
```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C autochannel -n KBA-Automobile --peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c '{"function":"CreateCar","Args":["Car-01","Tata","Nexon","White","Factory-1","22/07/2023"]}'
```
```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C autochannel -n KBA-Automobile --peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c '{"function":"CreateCar","Args":["Car-02","Tata","Tiago","Blue","Factory-1","22/07/2023"]}'
```

## Query – ReadCar
```
peer chaincode query -C autochannel -n KBA-Automobile -c '{"function":"ReadCar","Args":["Car-01"]}'
```
## Environment variables for Org2:

```
export CORE_PEER_LOCALMSPID=Org2MSP

export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt

export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp

export CORE_PEER_ADDRESS=localhost:9051
```

## Export order details
```
export MAKE=$(echo -n "Tata" | base64 | tr -d \n)

export MODEL=$(echo -n "Tiago" | base64 | tr -d \n)

export COLOR=$(echo -n "Blue" | base64 | tr -d \n)

export DEALER_NAME=$(echo -n "Popular" | base64 | tr -d \n)
```

## Invoke – CreateOrder
```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile $ORDERER_CA -C autochannel -n KBA-Automobile --peerAddresses localhost:7051 --tlsRootCertFiles $ORG1_PEER_TLSROOTCERT --peerAddresses localhost:9051 --tlsRootCertFiles $ORG2_PEER_TLSROOTCERT -c '{"Args":["OrderContract:CreateOrder","ORD-01"]}' --transient "{\"make\":\"$MAKE\",\"model\":\"$MODEL\",\"color\":\"$COLOR\",\"dealerName\":\"$DEALER_NAME\"}"
```
## Query – ReadOrder
```
peer chaincode query -C autochannel -n KBA-Automobile -c '{"Args":["OrderContract:ReadOrder","ORD-01"]}'
```

## Environment variables for Org1:
```
export CORE_PEER_LOCALMSPID=Org1MSP

export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt

export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp

export CORE_PEER_ADDRESS=localhost:7051
```
## Matching orders
```
peer chaincode query -C autochannel -n KBA-Automobile -c '{"Args":["GetMatchingOrders", "Car-02"]}'
```
## Match order
```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile $ORDERER_CA -C autochannel -n KBA-Automobile --peerAddresses localhost:7051 --tlsRootCertFiles $ORG1_PEER_TLSROOTCERT --peerAddresses localhost:9051 --tlsRootCertFiles $ORG2_PEER_TLSROOTCERT -c '{"function":"MatchOrder","Args":["Car-02","ORD-01"]}'
```
## Environment variables for Org3:
```
export CORE_PEER_LOCALMSPID=Org3MSP

export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org3.example.com/peers/peer0.org3.example.com/tls/ca.crt

export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org3.example.com/users/Admin@org3.example.com/msp

export CORE_PEER_ADDRESS=localhost:11051
```
## Register car

```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile $ORDERER_CA -C autochannel -n KBA-Automobile --peerAddresses localhost:7051 --tlsRootCertFiles $ORG1_PEER_TLSROOTCERT --peerAddresses localhost:9051 --tlsRootCertFiles $ORG2_PEER_TLSROOTCERT -c '{"function":"RegisterCar","Args":["Car-02","Bob","KL-01-7777"]}'
```
## History Query

```
peer chaincode query -C autochannel -n KBA-Automobile -c '{"Args":["GetCarHistory", "Car-02"]}' --peerAddresses localhost:9051 --tlsRootCertFiles $ORG2_PEER_TLSROOTCERT
```




