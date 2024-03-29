# iotflows python module

https://iotflows.com

IoTFlows Open Source Python WebSocket SDK.

With this tool you can:
1. Publish secure real-time data streams.
2. Subscribe to the data streams and access real-time data on your web/mobile/IoT apps.
3. Publish alerts to the alert channels with a defined severity level. Subscribers will get notified in the form of SMS/Email/Push.
4. Define cloud actions that can be called from other IoT devices/web applications.

## Installation
Use `pip3` to install the iotflows python module:

### Prerequisite
```
sudo pip3 install pathlib2
```

### IoTFlows Module
```
sudo pip3 install iotflows
```

Note: only Python3 is supported for this module.

## Usage

### Initialization
This function will create and initialize an IoTFlows instance.

```python
import iotflows.realtime as iotflowsRT
IoTFlows = iotflowsRT.init('CLIENT_ID', 'CLIENT_SECRET')
```

Make sure to change `CLIENT_ID` and `CLIENT_SECRET` with the proper credentials obtained from IoTFlows console. 
These credentials can be either one of these options:
1. A [Device Client](https://docs.iotflows.com/real-time-data-streams-alerts-and-actions/create-a-device-api-key) that has permission to interact with the resources available in its project, or
2. An [Organization IoT API KEY](https://docs.iotflows.com/cloud-node-red-servers/subscribe-and-publish-to-real-time-data-streams#create-an-iot-api-key) that can have read-only or read/write permissions to the entire organization resources
3. A [User Client](https://rest-api-docs.iotflows.com/#tag/Users/paths/%7E1v1%7E1users%7E1authorize/get) that is authorized to interact with the permitted resources of the user. This option is most useful when you need to build a web or mobile app. For this option, you need to register your Application in IoTFlows and authenticate users using [OAuth2](https://oauth.net/2). With the obtained [JWT](https://jwt.io/), you can perform a [Basic authentication](https://en.wikipedia.org/wiki/Basic_access_authentication) HTTP request to generate a User Client.

---

### Publish data stream
To publish a real-time data stream, you need to pass these parameters in a json object:

- data_stream_uuid: the uuid of the data stream
- data: the data to be published to the data stream

Read more:
- [How to create a data stream](https://docs.iotflows.com/iotflows-platform/creating-a-data-stream)

Example:
```python
IoTFlows.publish(
    data_stream_uuid='ds_xxxxxxxxxxxxxxxxxxxxxxx', 
    data='Hello World!')
```

---

### Subscribe to data stream
To listen to real-time data streams that are published, you need to define the following parameters in a json object:

- data_stream_uuid: data stream uuid
- qos (optional): quality of service 0, 1, or 2 (0: At most once, 1: At least once, 2: Exactly once)
- callback: handler function to be called when data received

Example:
```python
def handlerFunction(topic, payload):
    print('received new payload!!!')
    print(payload)

IoTFlows.subscribe(
    data_stream_uuid = 'ds_xxxxxxxxxxxxxxxxxxxxxxx',        
    qos = 2,
    callback = handlerFunction)
```

---

### Publish an alert
To publish an alert, you need to pass these parameters in a json object:

- alert_channel_uuid: the uuid of the alert channel
- severity_level: the severity level of the alert. It can be MAJOR, MINOR or INFORMATIVE
- subject: the subject of the alert
- description: the description/message of the alert

Read more:
- [How to create an alert channel](https://docs.iotflows.com/iotflows-platform/alert-channel#creating-an-alert-channel)

Example:
```python
IoTFlows.alert(
    alert_channel_uuid = 'ac_xxxxxxxxxxxxxxxxxxxxxxxx',
    severity_level = 'MINOR',
    subject = 'Water Leak',
    description = 'Water leackage detected in Site A.')
```
---

### Define a cloud action
To define a cloud action that can be called from other IoT/web applications, you need to define the following parameters in a json object:

- action_uuid action uuid
- qos (optional): quality of service 0, 1, or 2 (0: At most once, 1: At least once, 2: Exactly once)
- callback: handler function to be called when action gets executed

Read more:
- [How to create an action](https://docs.iotflows.com/iotflows-platform/creating-an-action)

Example:
```python
def controlPump(topic, payload):
    print('received new command!')
    print(payload)

IoTFlows.defineAction(
    action_uuid = 'da_xxxxxxxxxxxxxxxxxxxxxxxxx',        
    qos = 2,
    callback = controlPump)
```

---

### Call/Execute a cloud action
To publish an alert, you need to pass these parameters in a json object:

- action_uuid: the uuid of the action
- payload: the payload to be published to the action

Example:
```python
IoTFlows.callAction(
    action_uuid='da_xxxxxxxxxxxxxxxxxxxxxxxxx', 
    data='Turn on!')
```
