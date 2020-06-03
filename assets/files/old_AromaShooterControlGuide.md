4. How do I communicate with other Aroma Shooters through the Internet?

On the final day of Hackaroma, finalists will livestream a demo of their proof of concept. As part of this demo, finalists are expected to optimize their code such that all spectators with Aroma Shooters may experience their project's scents in realtime. Wow! If this seems a little tricky, we'll be happy to walk you through the process one-by-one.

![Flow of controlling Aroma Shooter via Internet](/assets/images/MQTT4AS.png)

​                                                   *Aroma Shooter Internet-based control flow*

(1) You will get a registered list of Aroma Shooters from the Hackaroma team.

(2) Your systems will use the list as input so that every time your device diffuses, it will send diffusion requests to all Aroma Shooters in the list. Between your code and the Aroma Shooters is an AWS IoT Broker which will help you communicate with the devices over the Internet. This broker uses an MQTT protocol.

(3) Here is an example written in **Python** using the AWS IoT [Python SDK](https://github.com/aws/aws-iot-device-sdk-python). This process should be similar to other AWS IoT-supported platforms and languages. 

- Check if your systems satisfy the minimum requirements listed in the AWS IoT Python SDK repository.

- Install the AWSIoTPythonSDK package.

- Add and run the following code to connect to the AWS IoT Broker.

  ```python
    from AWSIoTPythonSDK.MQTTLib import AWSIoTMQTTClient
  
    # For certificate based connection
    myMQTTClient = AWSIoTMQTTClient(Your_client_name_could_be_whatever)
  
    # For TLS mutual authentication
    myMQTTClient.configureEndpoint(provided_endpoint, 8883)
  
    # configure credentials
    # myMQTTClient.configureCredentials("YOUR/ROOT/CA/PATH", "PRIVATE/KEY/PATH", "CERTIFICATE/PATH")
    myMQTTClient.configureCredentials(PEM_file, PEM_key_file, certificate_file)
  
    # infinite offline publish queueing
    myMQTTClient.configureOfflinePublishQueueing(-1)
  
    # draining: 2Hz
    myMQTTClient.configureDrainingFrequency(2)
  
    # 10 secs
    myMQTTClient.configureConnectDisconnectTimeout(10)
  
    # 5 secs
    myMQTTClient.configureMQTTOperationTimeout(5)
  
    # connect to the AWS IoT Broker
    myMQTTClient.connect()
  ```

- In the above source code, please replace the placeholder *Your_client_name_could_be_whatever*. The following information will be provided by us:

  - *provided_endpoint*
  - *PEM_file*
  - *PEM_key_file*
  - *certificate_file*

- Once you successfully connect to the broker, trigger diffusions using code similar to the following (edit the ASN2 number for each Aroma Shooter on the list we provide):

  ```python
    topic = "aromajoin/aromashooter/ASN2A00002/command/diffuse"
    durationInMillis = 3000
    channel = 3
    intensity = 100
    payload = "{ \"duration\": " + str(durationInMillis) + ", \"channel\": " + str(channel) + ", \"intensity\": " + str(intensity) + ", \"booster\": false}"
    myMQTTClient.publish(topic, payload, 0)
  ```

- The following items are parameters that you can change for your own purposes:

  - *ASN2A00002*: this should be replaced by the serial number of the Aroma Shooter you want to control.
  - *durationInMillis*: the duration of diffusion time in milliseconds.
  - *channel*: the Aroma Shooter port number, from 1 to 6, corresponding to the scent you want to diffuse.
  - *intensity*: ranges from 0 to 100. This indicates the strength an Aroma Shooter will diffuse. *100* represents the strongest level.