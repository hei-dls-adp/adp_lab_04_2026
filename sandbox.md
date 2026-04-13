

:warning:

Attention,
Il y a un problème en écriture

De mon côté, j'ai du, pour une écriture simple, ajouter le type au payload.

Est-ce possible de n'envoyer que le payload ?

```js
var newMsg = msg;
var ParamIndex = msg.payload.ID - 2000;
var NewTopic = flow.get("LrealCmdTopic");
NewTopic = NewTopic.concat('/', ParamIndex.toString(),'/Value');

// newMsg.payload = msg.payload.Value;
newMsg.payload = {type:"double",value:msg.payload.Value};
// newMsg.topic = NewTopic;
newMsg.path = NewTopic;

return newMsg;
```

```js
plc/app/Application/sym/PackTag/Command/Parameter_Lreal/2/Value
```

```js
{"ID":2001,"Name":"Axes Velocity","Unit":"m/s","Value":0.05}

newMsg.payload

```

```js
plc/app/Application/sym/PackTag/Command/Parameter_Lreal
```
Peut-être avec ceci ?

```js
newMsg.path = plc/app/Application/sym/PackTag/Command/Parameter_Lreal/1
newMsg.payload = {type:object,{{"ID":2001,"Name":"Axes Velocity","Unit":"m/s","Value":0.05}}};
```

```js
newMsg.payload = {"type:"object","value":{"value":[{"ID":2001,"Name":"Axes Velocity","Unit":"m/s","Value":0.05},{"ID":2002,"Name":"Axes Acceleration","Unit":"m/s2","Value":0.52},{"ID":2003,"Name":"Axes Deceleration","Unit":"m/s2","Value":0.56},{"ID":2004,"Name":"Axes Jerk","Unit":"m/s3","Value":1},{"ID":2005,"Name":"Position Camera X","Unit":"mm","Value":-130},{"ID":2006,"Name":"Position Camera Y","Unit":"mm","Value":130},{"ID":2007,"Name":"Position Camera Z","Unit":"mm","Value":-54},{"ID":2008,"Name":"X Out Of Camera","Unit":"mm","Value":-80},{"ID":0,"Name":"","Unit":"","Value":0},{"ID":0,"Name":"","Unit":"","Value":0},{"ID":0,"Name":"","Unit":"","Value":0},{"ID":0,"Name":"","Unit":"","Value":0},{"ID":0,"Name":"","Unit":"","Value":0},{"ID":0,"Name":"","Unit":"","Value":0},{"ID":0,"Name":"","Unit":"","Value":0},{"ID":0,"Name":"","Unit":"","Value":0},{"ID":0,"Name":"","Unit":"","Value":0},{"ID":0,"Name":"","Unit":"","Value":0},{"ID":0,"Name":"","Unit":"","Value":0},{"ID":0,"Name":"","Unit":"","Value":0}]},"schema":"types/plc/app/Application/ArrayOfHEVS_PackTag_Parameter_Lreal","responseType":"read"}
```

Cela ne marche pas
```js
var newMsg = {}

// newMsg.path = "plc/app/Application/sym/PackTag/Command/Parameter_Lreal/1
newMsg.payload = {"object":{ "ID": 2001, 
                             "Name": "Axes Velocity", 
                             "Unit": "m/s", 
                             "Value": 0.05 }};

return msg;
```