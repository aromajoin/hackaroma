# The world's first scent-inspired hackathon

[English](README.md) / [日本語](README-JP.md)

Hackaroma [Official Site](https://www.aromajoin.com/hackaroma)

1. Overview
   - Applications open May 13th. Livestreaming demos June 19th.
   - Official rules: [PDF](https://drive.google.com/file/d/1pwpCksr0kRWzzq3HsPF0bcMUr-uwWLaL/view)
   - Proposal Screening Round (5/13 ~ 5/28):
     - Proposal guidelines, suggestions, things to know → [A short guide](https://paper.dropbox.com/doc/Perfecting-your-Hackaroma-Proposal--AzWa4BFYALfWgkcztSeRTRhaAQ-8VblQZyV0ehKdyAmCSeOV)
     - [Sample Proposal](https://www.dropbox.com/s/9xcwgmslopemi94/200508_HackaromaProposalTemplate.pdf?dl=0)
   - Development Round (5/28 ~ 6/19):
     - Six finalists (individuals or teams) will develop their ideas. We will do what we can to get an Aroma Shooter dev kit to you quickly, however finalists may start development with or without a device. Dev kits will include a set of cartridges. The plan is to send the same set of cartridgs to all finalists. The number of cartridges in each set is still to be determined, though there will be at least six.

     - On the final day, June 19th (Friday) JST, at a TBD time, we will host an online pitch event where finalists demo their projects such that pre-selected audiencemembers with Aroma Shooters will be able to smell the scents in the demo simultaneously. This may in fact be **the world's first scent data livestreaming**.

     - Aroma Shooters are controlled through the Internet using AWS IoT, of which SDKs are available for iOS, Android, Java, Javascript, Python, C++, and more. Learn more [here](https://docs.aws.amazon.com/iot/latest/developerguide/iot-sdks.html). More details and guides specific to the Aroma Shooter coming soon.

2. Example Aroma Shooter solutions:
   - [Aroma VR](https://www.dropbox.com/s/9xse6isg22fhuw9/200109_VRHeroVideo.mp4?dl=0)
   - [Scent Walkman](https://www.youtube.com/watch?v=r9MUcdwxsR4)
   - [Productivity Booster](https://www.youtube.com/watch?v=p1f5A-vXAv8)
   - [Aroma Signage](https://aromajoin.com/solutions/aroma-signage)
   - [Others](https://aromajoin.com/solutions/arts-and-science)

3. What types of scents will be available for the development round?

   - Each finalist will receive the following with their Aroma Shooter kit:

     - Imitation Chanel #5
     - Mango
     - Coffee
     - Lavender
     - Forest
     - Hinoki
     - Spearmint
     - お香 (Incense)
     - Lemon
     - Fresh Cut Grass
     - Burnt Rubber
     - Ocean
     - Caramel

   - There are 13 (we have recently decided to add Caramel to the list) here, of which you may use 6 in one Aroma Shooter simultaneously. What do you think? Do these spark your creativity?

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
