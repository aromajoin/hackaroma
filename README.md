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
     - Six finalists (individuals or teams) will develop their ideas. We will do what we can to get an Aroma Shooter dev kit to you quickly, however finalists may start development with or without a device. Dev kits will include a set of cartridges. The plan is to send the same set of cartridges to all finalists, the list of which you'll find below.

     - On the final day, June 19th (Friday) JST, at a TBD time, we will host an online pitch event where finalists demo their projects such that pre-selected audience members with Aroma Shooters will be able to smell the scents in the demo simultaneously. This may in fact be **the world's first scent data livestreaming**.

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

4. How do I communicate with other Aroma Shooters through the Internet? ([Old setup](/assets/files/old_AromaShooterControlGuide.md))

- HTTP-based Aroma Shooter control flow.

 ![Flow of controlling Aroma Shooter via Internet](/assets/images/HTTP4AS.png)

- Steps

  - Connect your Aroma Shooter to a local network which is connected with the internet. If you don't know how, please refer to [this document](https://github.com/aromajoin/controller-http-api).
  - Send HTTP request to control.　※ *The device will automatically disconnect after roughly 1 hour without any receiving request. If it happens, please plug the device out from the power source and then plug it in again.*

- Unicast vs. Broadcast

  - By specifying the type of casting as unicast, you control **only** your device.
  - By specifying the type of casting as broadcast, you control all devices in a pre-registered list (including your own device). 
  - During the **development** time, both unicast and broadcast control only the device you are keeping.
  - There is a **rehearsal** time (we will meet with each finalist separately), when broadcast controls a set of testing device.
  - On the day of the final pitch, broadcast controls all registered devices, including yours, ours, judges' and probably others'. So on the day of final pitch, **except for demo, please DO NOT use broadcast**.

- Which credentials are required?

  - API Key: this will be mailed to each of you.
  - Private Key: this will be mailed to each of you. Each finalist has a distinct key.

- What is the design of the HTTP API?

  - **Warning!**: A continuous diffusing of more than 10 secs. from one channel might break Aroma Shooters. For example if you want to diffuse in 15 seconds from channel 3, then a structure like: diffuse 5 seconds, stop 5 seconds, and diffuse 5 seconds, is recommended.

  - URL: https://lvxcf3yn35.execute-api.us-west-2.amazonaws.com/prod/as2/control

  - Type of request: POST

  - Header must include the API Key.

  - Attachment: JSON data, which includes the following fields

    - For diffusion:

      ```json
        {
            "private_key":"your_private_key_that_we_provide",
            "casting":"unicast/broadcast",
            "type":"diffuse",
            "duration":"time_to_diffuse_in_milisecond",
            "booster":"true/false", // to toggle on and off the non scent channel,
            "channel":"1~6",
            "intensity":"0-100"
        }
        ```
    - Example:

      ```json
        {
            "private_key":"place_holder",
            "casting":"unicast",
            "type":"diffuse",
            "duration":"3000",
            "booster":"true",
            "channel":"1",
            "intensity":"100"
        }
        ```

        

    - For stopping diffusion:

      ```json
      {
          "private_key":"your_private_key_that_we_provide",
          "casting": "unicast/broadcast",
          "type": "stop",
          "channel": "1~6"
      }
      ```

    - Example:

      ```json
      {
          "private_key":"place_holder",
          "casting":"broadcast",
          "type":"stop",
          "channel":"1"
      }
      ```

      

- How can you test sending HTTP request?

  - Highly recommend using Postman
   ![Postman url and header set up](/assets/images/postman_header.png)
   (Set up URL and header)
   ![postman http data set up](/assets/images/postman_data.png)
   (Set up JSON data)

- How many times can I send requests per day?

  - This API server is running on AWS Serverless framework, which is a paid service. We prepared it for maximum roughly 5000 requests/day.
  - This is the total number of requests this server can receive per one day. So if you find this problem, please try to wait until the following day when counting starts over.
  - Roughly, each of you has more than 800 requests/day. Please keep this in mind.

- Finally, should you have any technical question, please feel free to let us know via hackaroma@aromajoin.com.
