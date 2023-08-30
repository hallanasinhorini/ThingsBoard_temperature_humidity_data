# Streaming Live Data to ThingsBoard

****Tech Stack: Python, MQTT protocol, ThingsBoard, Firebase, rest API , Docker****

This project will use the MQTT protocol to produce temperature and humidity data, ensure that the data produced by the MQTT protocol is published correctly to ThingsBoard, and finally, use a real-time database in Firebase and send the temperature and humidity data to it.

1. Navigate to the Project_ThingsBoard_Docker folder and run the command below to initialize the Mosquitto container:

docker-compose up

2. install the Paho MQTT Python client library locally:

pip install paho-mqtt

3. Navigate to the Project_ThingsBoard_MQTT/ThingsBoard folder and run the command below to initialize the ThingsBoard container:

docker-compose up

4.  Run the TBPublish.py file.

5. Navigate to http://localhost:8080/. Log in to ThingsBoard using the credentials below:

Login: tenant@thingsboard.org
Password: tenant

6. In ThingsBoard, from the menu on the left, select “Devices”. You should see an existing device called DHT11 Demo Device. This device publishes data produced by an MQTT protocol to ThingsBoard. 

7. Realtime database in Firebase, add a field in the database titled temperature and initialize the corresponding field to zero.

8. Navigate to the Root Rule Chain in ThingsBoard. Add a “rest API call” node and name it Firebase. In the “Endpoint URL pattern” field, paste the link to your Realtime database from Firebase followed by "/temperature.json". Connect the “Firebase” node to the “Message Type Switch” node. Add “Post telemetry” as the link label.

9. Navigate to Firebase and create a new field titled alarms and initialize the corresponding field to zero.

10. Navigate to the ThingsBoard home page. Create a new rule chain titled “AlarmToFirebase”, “CreateAndClearAlarms” and “TempToFirebase”.

11. Open the “TempToFirebase” rule chain. Add a “rest API call” node and name it “TempToFirebase”. Replace the default link. 

12. Open the “CreateAndClearAlarms” rule chain. Add a “rule chain” node. Title this node “AlarmToFirebase” and select AlarmToFirebase as the rule chain. Select “Add” to add the AlarmToFirebase node to your Rule Engine.

13. Connect the CreateAlarm and AlarmToFirebase nodes. Add “Created” as the link label.

14. Add another “rule chain” node to the CreateAndClearAlarms rule chain. Title this node “TempToFirebase” and select TempToFirebase as the rule chain. Select “Add” to add the TempToFirebase node to your Rule Engine

15. Connect the MaxTemp and TempToFirebase nodes. Add “True” as the link label.

16. Open the Root Rule Chain in ThingsBoard. Add a “rule chain” node. Name it “CreateAndClearAlarm” and select CreateAndClearAlarm as the rule chain. Select “Add” to add the CreateAndClearAlarm node to your Rule Engine.

17. Connect the SaveTimeseries and CreateAndClearAlarm nodes. Add “Success” as the link label.

