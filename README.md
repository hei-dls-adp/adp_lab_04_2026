<h1 align="left">
  <br>
  <img src="./img/hei-en.png" alt="HEI-Vs Logo" width="350">
  <br>
  HEI-Vs Engineering School - DLS / Automation in Development and Production
  <br>
</h1>

Course DLS/ADP


Author: [Cédric Lenoir](mailto:cedric.lenoir@hevs.ch)

# Lab 04, Management of CSV and JSON files.

## We have these nodes

<div align="center">
<figure>
  <img src="./img/Node-RED_Parser.png"
     alt="Image lost: Node-RED_Parser"
     width="200"><br>
  <figcaption>Node-RED parser</figcaption>
</figure>
</div>

**and**

<div align="center">
<figure>
  <img src="./img/Node-RED_Storage.png"
     alt="Image lost: Node-RED_Storage"
     width="200"><br>
  <figcaption>Node-RED storage</figcaption>
</figure>
</div>

---

### We will use these nodes from Parser

#### CSV
To convert to from **CSV** format

<div align="center">
<figure>
  <img src="./img/Node-RED_Parser_csv.png"
     alt="Image lost: Node-RED_Parser_csv"
     width="200"><br>
  <figcaption>Node-RED parser csv</figcaption>
</figure>
</div>

#### JSON
To convert to from **JSON** format

<div align="center">
<figure>
  <img src="./img/Node-RED_Parser_json.png"
     alt="Image lost: Node-RED_Parser_json"
     width="200"><br>
  <figcaption>Node-RED parser json</figcaption>
</figure>
</div>

#### XML
To convert to from **XML** format

<div align="center">
<figure>
  <img src="./img/Node-RED_Parser_xml.png"
     alt="Image lost: Node-RED_Parser_xml"
     width="200"><br>
  <figcaption>Node-RED parser xml</figcaption>
</figure>
</div>

---

### ... and these from Storage

#### Write file

<div align="center">
<figure>
  <img src="./img/Node-RED_Write_File.png"
     alt="Image lost: Node-RED_Write_File"
     width="200"><br>
  <figcaption>Node-RED write file</figcaption>
</figure>
</div>

#### Read file

<div align="center">
<figure>
  <img src="./img/Node-RED_Read_File.png"
     alt="Image lost: Node-RED_Read_File"
     width="200"><br>
  <figcaption>Node-RED read file</figcaption>
</figure>
</div>

#### Watch file

<div align="center">
<figure>
  <img src="./img/Node-RED_Watch_File.png"
     alt="Image lost: Node-RED_Watch_File"
     width="200"><br>
  <figcaption>Node-RED watch file</figcaption>
</figure>
</div>

---

## First task

Subscribe ot motion done, ``bool8`` and status ``string`` from PLC and display them in Motion status of UI.

```js
// Path
plc/app/Application/sym/PRG_Unit/emRobot/_MotionDone
plc/app/Application/sym/PRG_Unit/emRobot/_MotionStatusString
```
Using **link in nodes** ``Start Linear``, ``Start Pick`` and _motionDone from PLC, build this UI. It will be used to detect when to start and stop writing motion positions in a file.

<div align="center">
<figure>
  <img src="./img/MotionActiveDone.png"
     alt="Image lost: MotionActiveDone"
     width="400"><br>
  <figcaption>Motion started or done</figcaption>
</figure>
</div>

---

## Second task

Build a button and display an object in the debug window using a function with this code.

```js
// get a timestamp
const event = new Date();
// convert to local time
msg.payload = {time : event.toLocaleTimeString("en-US", { timeZone: "Europe/Berlin" }),
               action : "Reset Button"};

return msg;
```

:bulb: Processing time and date can be very complicated, mainly due to the very large number of possible options.

Then you can use a json parser node to build a json string and write it in a file to record each time you press a button to start a motion.

**Use this path for write file.**

```js
// Take care to use your name and first name.
C:\Users\yourfirstname.yourname\Documents\Adp\Lab_04_2026\LogEvent.json
```
You have to set Action as **append to file**.

**Check your work and open the json file with Notepad++**


<b style='color:red;'>Now you are able to log any event from your UI Dashboard in a file.</b>

---

## Third task.
Write this array of struct from the PLC to a Json file and restore themm
you need a Write and a Read button.

```js
// From / To PLC path
plc/app/Application/sym/PackTag/Command/Parameter_Lreal
```

Use the example of process below to write read the xml file.

<div align="center">
<figure>
  <img src="./img/ReadWriteJson.png"
     alt="Image lost: ReadWriteJson"
     width="600"><br>
  <figcaption>Write read json file process</figcaption>
</figure>
</div>

:bulb: I suggest you to use an intermediate variable in the flow.

```js
// Take care to use your name and first name.
C:\Users\yourfirstname.yourname\Documents\Adp\Lab_04_2026\CmdParameters.json
```

<b style='color:red;'>Now you are able to store restore a set of parametes for a machine with standard json file.</b>

**Please, check your system be editing the xml file !!!**

##

## Remember Subflows
Here you have to write a subflow to log a button


---

## Fourth task
Based on this example of CtrlX Core, [Visualize CSV Data with Node-RED on ctrlX CORE](https://community.boschrexroth.com/ctrlx-automation-how-tos-qmglrz33/post/visualize-csv-data-with-node-red-on-ctrlx-core-Nj5gNO0CXwmFOhz).

---

## Fifth task

### Create a single message from separate streams of messages

Based on this link [Create a single message from separate streams of messages](https://cookbook.nodered.org/basic/join-streams).

#### Problem

You have messages arriving from different sources that you need to combine into a single message.

For example, you have three different sensors publishing values and you want to insert them into a database as a single entry.


#### Solution

Give each stream a unique ``msg.topic`` value and use the **Join** node to group them into a single message.
**Example**

<div align="center">
<figure>
  <img src="./img/CreateSingleMessageFromSeparateStreams.png"
     alt="Image lost: CreateSingleMessageFromSeparateStreams"
     width="500"><br>
  <figcaption>Create single message from separate streams</figcaption>
</figure>
</div>

**Flow**
```js
[{"id":"8ccddb9a.a55f38","type":"inject","z":"ac14500e.2c57d","name":"temperature","topic":"temperature","payload":"10","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":110,"y":1760,"wires":[["47b769c5.cb0e28"]]},{"id":"47b769c5.cb0e28","type":"join","z":"ac14500e.2c57d","name":"","mode":"custom","build":"object","property":"payload","propertyType":"msg","key":"topic","joiner":"\\n","joinerType":"str","accumulate":false,"timeout":"","count":"3","reduceRight":false,"reduceExp":"","reduceInit":"","reduceInitType":"","reduceFixup":"","x":310,"y":1800,"wires":[["f9afb265.b11b7"]]},{"id":"f9afb265.b11b7","type":"debug","z":"ac14500e.2c57d","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":470,"y":1800,"wires":[]},{"id":"2d269127.4f04ce","type":"inject","z":"ac14500e.2c57d","name":"humidity","topic":"humidity","payload":"","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":100,"y":1800,"wires":[["47b769c5.cb0e28"]]},{"id":"d6fbe805.0e4628","type":"inject","z":"ac14500e.2c57d","name":"pressure","topic":"pressure","payload":"999","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":100,"y":1840,"wires":[["47b769c5.cb0e28"]]}]
```

#### Discussion

In the example flow, each **Inject** node represents a different source of messages. They each set a unique ``msg.topic`` value so they can be identified later in the flow.

The **Join** node has been configured in manual mode to create a key/value object using ``msg.topic`` as the key name. As we know there are three separate streams of messages to join, the node has been to configure to send on a message when it receives that number of parts.

This means it will send on a message each time it receives at least one message from three different topics - using the most recent value from each topic.

```js
{
    "temperature":10,
    "humidity":0,
    "pressure":999
}
```

The node has further options to change its behaviour that have not been used in this recipe. For example, a timeout can be set to ensure it sends *something* in case one of the sensors stops sending values. If that is a concern, you may consider this recipe for providing a placeholder value.

There is a collection of recipes for how to use Node-RED in this [Node-RED Cookbook](https://cookbook.nodered.org/#flow-control). In the context of this lab, you could have a look on [Working with data formats](https://cookbook.nodered.org/#working-with-data-formats);

---

## Sixth Task

### Send placeholder messages when a stream stops sending
[Based on this page of Node-RED Cookbook](https://cookbook.nodered.org/basic/trigger-placeholder).

#### Problem

You have a stream of messages coming from a sensor at regular intervals. If the sensor stops sending messages, you want to send placeholder messages at the same rate.

For example, the sensor data may be feeding a Dashboard chart. If the sensor stops sending, the chart will stop updating. So placeholder messages are needed for the chart to update with a **0** value to highlight the sensor has stopped.

#### Solution

Use the **Trigger** node to detect when a message has not arrived after a defined interval and a second **Trigger** node to send the placeholder messages at a regular interval.

**Example**

<div align="center">
<figure>
  <img src="./img/SendPlaceHolderMessagesWhenAStreamStops.png"
     alt="Image lost: SendPlaceHolderMessagesWhenAStreamStops"
     width="500"><br>
  <figcaption>Send placeholder messages when a stream stops sending</figcaption>
</figure>
</div>

```js
[{"id":"9ccdf268.c96ff","type":"inject","z":"ac14500e.2c57d","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":100,"y":1660,"wires":[["38950a5.28d15f6","2c532f67.0330e"]]},{"id":"38950a5.28d15f6","type":"debug","z":"ac14500e.2c57d","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":610,"y":1660,"wires":[]},{"id":"2c532f67.0330e","type":"trigger","z":"ac14500e.2c57d","op1":"reset","op2":"true","op1type":"str","op2type":"bool","duration":"2","extend":true,"units":"s","reset":"","bytopic":"all","name":"","x":260,"y":1700,"wires":[["e4e42b96.97a338"]]},{"id":"e4e42b96.97a338","type":"trigger","z":"ac14500e.2c57d","op1":"0","op2":"0","op1type":"num","op2type":"str","duration":"-2","extend":false,"units":"s","reset":"reset","bytopic":"all","name":"","x":420,"y":1700,"wires":[["38950a5.28d15f6"]]}]
```

#### Discussion

In the example flow, the top branch represents the normal flow of the messages, from the **Inject** node to the **Debug** node.

The messages also get passed to the first **Trigger** node on a second branch of the flow. That node is configured to initially send a payload of ``reset``, then to wait for 2 seconds before sending a timeout message. The option to extend the delay if new messages arrive is also selected. This means as long as messages continue to arrive, the node will not do anything. Once 2 seconds passes after the last message to arrive, it will send on the timeout message.

The timeout message feeds into a second **Trigger** node. This node is configured to send on **0** every two seconds and feeds back into the top branch. The node is also configured to stop sending if it receives a ``msg.payload`` of ``reset``. As this is the initial message sent by the first **Trigger** node when it receives a sensor message, this will cause the second **Trigger** node to be reset when the sensor resumes sending its own messages.

---
## Seventh task

We will use a trigger to start record of X, Y and Z positions of a robt on a csv file. You can then add buttons to reset start the trigger or use the [first task](#first-task).

**The trigger**

<div align="center">
<figure>
  <img src="./img/BuildTrigger.png"
     alt="Image lost: BuildTrigger"
     width="500"><br>
  <figcaption>Build a trigger</figcaption>
</figure>
</div>

```json
[{"id":"90c57959f492d204","type":"group","z":"fda828b9cf218447","style":{"stroke":"#999999","stroke-opacity":"1","fill":"none","fill-opacity":"1","label":true,"label-position":"nw","color":"#a4a4a4"},"nodes":["d72221f60c2de6cb","ecaa35f7c7873f67","d37fc4d6a47ba0ef","b7206323cb5b7095","713f9783df8256a1"],"x":34,"y":939,"w":692,"h":162},{"id":"d72221f60c2de6cb","type":"trigger","z":"fda828b9cf218447","g":"90c57959f492d204","name":"","op1":"0","op2":"0","op1type":"num","op2type":"str","duration":"-500","extend":false,"overrideDelay":false,"units":"ms","reset":"reset","bytopic":"all","topic":"topic","outputs":1,"x":380,"y":1000,"wires":[["d37fc4d6a47ba0ef"]]},{"id":"ecaa35f7c7873f67","type":"inject","z":"fda828b9cf218447","g":"90c57959f492d204","name":"","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":140,"y":980,"wires":[["d72221f60c2de6cb"]]},{"id":"d37fc4d6a47ba0ef","type":"debug","z":"fda828b9cf218447","g":"90c57959f492d204","name":"debug 27","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","statusVal":"","statusType":"auto","x":620,"y":1000,"wires":[]},{"id":"b7206323cb5b7095","type":"inject","z":"fda828b9cf218447","g":"90c57959f492d204","name":"","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"reset","payloadType":"str","x":150,"y":1020,"wires":[["d72221f60c2de6cb"]]},{"id":"713f9783df8256a1","type":"comment","z":"fda828b9cf218447","g":"90c57959f492d204","name":"Build a trigger with xx ms repetition","info":"","x":200,"y":1060,"wires":[]}]
```

Get axes positon using the trigger with paths of axes:

```js
// Robot axes
plc/app/Application/sym/PRG_Unit/emRobot/groupPosition_X
plc/app/Application/sym/PRG_Unit/emRobot/groupPosition_Y
plc/app/Application/sym/PRG_Unit/emRobot/groupPosition_Z
```

You can build a payload object on the model of the [second task](#second-task).

:bulb: you have to replace the action by the axes names and positions.

```js
// Take care to use your name and first name.
C:\Users\yourfirstname.yourname\Documents\Adp\Lab_04_2026\RobotPosition.csv
```

**Writing axes position**

<div align="center">
<figure>
  <img src="./img/WritingRobotPositions.png"
     alt="Image lost: WritingRobotPositions"
     width="500"><br>
  <figcaption>Writing robot positions</figcaption>
</figure>
</div>

:bulb: You will have to use the [fifth task](#fifth-task) to join the robot positions.

---

## Annexe
For JavaScript math functions, have a look on this page: [Mozilla Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)

---

## Bug
I have a problem with reading XML parameters.
Problem to solve...


Check that...

Write this array of struct from the PLC to a XML file and restore themm
you need a Write and a Read button.

```js
// From / To PLC path
plc/app/Application/sym/PackTag/Command/Parameter_Lreal
```

Use the example of process below to write read the xml file.

<div align="center">
<figure>
  <img src="./img/ReadWriteXmlProcess.png"
     alt="Image lost: ReadWriteXmlProcess"
     width="600"><br>
  <figcaption>Write read xml file process</figcaption>
</figure>
</div>

:bulb: I suggest you to use an intermediate variable in the flow.

```js
// Take care to use your name and first name.
C:\Users\yourfirstname.yourname\Documents\Adp\Lab_04_2026\CmdParameters.xml
```

<b style='color:red;'>Now you are able to store restore a set of parametes for a machine with standard xml file.</b>

**Please, check your system be editing the xml file !!!**

<b style='color:red;'>Maybe with the help of that ?</b>
C:\Users\cedric.lenoir\Documents\Git_Hub\adp_lab_04_2026\node_red_base\To Insert in your flows

<!-- End of document -->