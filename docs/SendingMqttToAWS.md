# Sending MQTT Messages to AWS using AT Command Firmware

This tutorial will show you how to connect the Sterling EWB to an Access Point and then send MQTT messages to AWS.

## Required Tools

   - [EWB dev kit](https://www.lairdconnect.com/wireless-modules/wifi-modules-bluetooth/sterling-ewb-iot-module) (Part No. **455-00030** or **455-00031**)
   - [TTL-232R-3V3](https://ftdichip.com/products/ttl-232r-3v3/) USB to UART Cable
   - Windows PC
   - WiFi Access Point

## Prerequisites

   - You have flashed the [AT Command Firmware](https://www.lairdconnect.com/wireless-modules/wifi-modules-bluetooth/sterling-ewb-iot-module#documentation) into the Sterling EWB dev kit. To flash the firmware, simply download it from our website, connect to J24 of the dev kit, and then run the ***flash_AT.bat*** file included with the firmware.

   - You have an AWS account with IoT Core Service.

   - You have a Windows PC with Python 3.x.x installed. This demo was done with v3.9.1

   - You have downloaded the [ATCommands_SampleApps](https://www.lairdconnect.com/documentation/command-set-python-sample-applications-sterling-ewb) from our website.

   - You have a WiFi access point to connect to the cloud.

     

## Setup

Supply power to the development board via J24. Connect the TTL-232R-3V3 cable to J7 as shown below and then connect the other end of the cable to the your Windows PC. Then use Windows Device Manager determine the COM port for the TTL-232R-3V3. For this tutorial, we will use ***COM30***.

   ![](../images/mqtt-aws/Setup.PNG)



## Steps

1. Login to your AWS IoT Service account and create a policy.

   - [ ] Navigate to ***Secure->Policies*** and then click ***Create a policy***.

     ![CreatPolicy1-edit](../images/mqtt-aws/CreatPolicy1-edit.PNG)

   

   - [ ] Create the policy as shown below. For ***Action*** enter ***iot: Connect, iot:Publish***.  For ***Resource ARN*** enter *****.  For ***Effect*** select ***Allow***. This policy will allow devices (e.g. Sterling EWB) to connect and publish MQTT messages.

     ![CreatPolicy2-edit](../images/mqtt-aws/CreatPolicy2-edit.PNG)	

   - [ ] If the policy was created successfully, you should see the screen below.

     ![CreatPolicy3-edit](../images/mqtt-aws/CreatPolicy3-edit.PNG)

     

2. Create a Thing (i.e. Sterling EWB Sensor that will send MQTT messages).

   - [ ] Navigate to ***Manage->Things*** and then click ***Create things***.

     ![](../images/mqtt-aws/CreatThing1-edit.PNG)

     

   - [ ] Select ***Create single thing*** and then click ***Next***.

     ![](../images/mqtt-aws/CreatThing2-edit.PNG)

     

   - [ ] Name the ***Thing*** as ***MySensor*** and then click ***Next***.

     ![CreatThing3-edit](../images/mqtt-aws/CreatThing3-edit.PNG)	

   - [ ] Select ***Auto-generate a certificate*** and then click ***Next***.

     ![](../images/mqtt-aws/CreatThing4-edit.png)

     

   - [ ] Attach the policy that we created to ***MySensor*** by selecting ***MyPolicy*** and then click ***Create thing***.

     ![CreatThing5-edit](../images/mqtt-aws/CreatThing5-edit.PNG)

     

   - [ ] Download the ***MySensor*** certificate, public and private keys, and the ***Amazon Root CA certificates*** into the ***examples*** folder of the [Python Samples Apps](https://www.lairdconnect.com/documentation/command-set-python-sample-applications-sterling-ewb). Note, for this tutorial, we really just need the ***MySensor*** certificate and private keys, but you may download all the certificates and keys in case you need them in the future.

     ![](../images/mqtt-aws/CreatThing6-edit.png)

     

     Rename the downloaded ***MySensor*** certificate as ***MySensor.pem.crt.*** Rename the public key as ***MySensor.public.pem.key***. Rename the private key as ***MySensor.private.pem.key***. 

     

   - [ ] ***MySensor*** is now created as shown below. Next click ***MySensor***.

     ![](../images/mqtt-aws/CreatThing7-edit.png)

     

3. Copy and save hostname/endpoint into a textfile.

   - [ ] Select the ***Interact*** tab and then click ***View Settings***.

     ![CopyHost1-edit](../images/mqtt-aws/CopyHost2-edit.png)

     

   - [ ] Copy the endpoint url and save into a textfile. We will need this later when we run our Python sample scripts. It will be used as the hostname.

     ![CopyHost3-edit](../images/mqtt-aws/CopyHost3-edit.PNG)

   

4. Setup the AWS MQTT Test Client.

   Navigate to ***Test->MQTT test client***. Enter ***#*** on the ***Topic filter***.  Expand ***Additional configuration*** and select ***Display payload as strings***. Then click ***Subscribe***. ***#*** should be added to ***Subscriptions***.
   
   ![TestMQTT](../images/mqtt-aws/TestMQTT.png)
   
   
   
5. Send MQTT messages from the EWB.

   - [ ] Open a ***cmd prompt*** on the ***ATCommands_SampleApps*** ***examples*** folder.

   - [ ] Connect to an access point using the ***join.py*** script.

     `join.py -u COM30 -s NameOfYourAP -p YourPassphrase`

   - [ ] Load the ***MySensor*** certifcate and private key into the EWB with the ***client_cert.py*** script.

     `client_cert.py -u COM30 --cert MySensor.pem.crt --key MySensor.private.pem.key`

   - [ ] Send a message with the ***mqtt.py*** script.

     `mqtt.py -u COM30 -p 8883 --host TheEndPointURLYouCopiedFromStep3 --ssl NoVerifyHost --topic test/topic --body Hello`

     ![CmdPrompt-Edit](../images/mqtt-aws/CmdPrompt-Edit.PNG)

   - [ ] The hello message is received on the AWS MQTT Test Client.

     ![](../images/mqtt-aws/MsgReceived.PNG)

   

## References

- [Sterling EWB Product Page](https://www.lairdconnect.com/wireless-modules/wifi-modules-bluetooth/sterling-ewb-iot-module)

- [Sterling EWB AT Command FW](https://www.lairdconnect.com/wireless-modules/wifi-modules-bluetooth/sterling-ewb-iot-module#documentation)

- [Python Sample Apps](https://www.lairdconnect.com/documentation/command-set-python-sample-applications-sterling-ewb)

- [AT Command Guide](https://www.lairdconnect.com/documentation/command-guide-sterling-ewb)

- [AT Command Quick Start Guide](https://www.lairdconnect.com/documentation/command-quick-start-guide-sterling-ewb)

- [Dev Kit User Guide](https://www.lairdconnect.com/documentation/hardware-dvk-guide-sterling-ewb)

  
  
  
