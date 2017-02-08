# Automotive Extension

This end point is for devices that support the automotive hw_specialty.

## Get supported buses

> Gather supported buses

```json
[
  {"bus_name":"vcan0"},
  {"bus_name":"can0"},
  {"bus_name":"can1"}
]
```

```javascript
app.get('/automotive/supported_buses', function (req, res) {
  //... Gather all capabile buses and return an array of bus_names ...
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify([ { bus_name: "vcan0" }, { bus_name: "can0" } ], null, 1));
})
```

Returns a unique name for all supported buses.  These names are used as targets for
the supported modules.

### HTTP Request

`GET http://example.com/api/automotive/supported_buses`

### Response Parameters

Returns an array of active buses.

Paramter | Default | Description
---------|---------|------------
bus_name | []      | Array of unique bus_name used as targets for modules

## Configure a bus

> Sets buad rate of CAN buses

```json
{
  "bitrate": 500000
}
```

```javascript
app.get('/automotive/:bus_name/config', function (req, res) {
  var bus = req.params.bus_name;
  //... Report the current bus configuration ...
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ bitrate: 500000 }, null, 1));
})
```

Sets sepcial configurations for a target bus returned by supported_buses.

### HTTP Request

`GET http://example.com/api/automotive/:bus_name/config`

### Response Parameters

The current configurations of the targetted bus_name

Paramter | Default | Description
---------|---------|------------
bitrate  | 0       | Numeric bitrate of the bus, example: 500000 for 500k baud

## Send a CAN Packet

> Send CAN Packets

```json
{
  "success": true
}
```

```javascript
app.get('/automotive/:bus_name/cansend', function (req, res) {
  var bus = req.params.bus_name;
  var id = req.query.id;
  var data = req.query.data;
  //... do the work...
})

```

Sends a CAN packet.  Only required if a canbus is supported by the hardware.

### HTTP Request

`GET http://example.com/api/automotive/:bus_name/cansend`

### Query Parameters

Required Parameters

Paramter | Default | Description
---------|---------|------------
id       | "0"     | Hex value of the CAN ID represented as a string. Example: "7DF"
data     | ""      | A string representing up ot 8 hex bytes.  Example "02FF09"

### Response Parameters

Returns success if the packet was able to be sent.

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected

## Send an ISOTP Packet and wait for response

> Handle ISO-TP Requests and responses

```json
/* Example of a packet response */
{  
   "Packets":[  
      {  
         "ID":"7E8",
         "DATA":[  "10", "14", "49", "02", "01", "31", "47", "31" ]
      },
      {  
         "ID":"7E8",
         "DATA":[  "21", "5A", "54", "35", "33", "38", "32", "36" ]
      },
      {  
         "ID":"7E8",
         "DATA":[  "22", "46", "31", "30", "39", "31", "34", "39" ]
      }
   ]
}

```

```javascript
app.get('/automotive/:bus_name/isotpsend_and_wait', function (req, res) {
  var bus = req.params.bus_name;
  var srcid = req.query.srcid;
  var dstid = req.query.dstid;
  var data = req.query.data;
  // Optional
  var timeout = req.query.timeout;
  var maxpkts = req.query.maxpkts;
  //... do the work...
})

```
Sends a packet in the ISO-TP format and then waits for an ISO-TP Styled response.

### HTTP Request

`GET http://example.com/api/automotive/:bus_name/isotpsend_and_wait`

### Query Parameters

Required Parameters

Paramter | Default | Description
---------|---------|------------
srcid    |         | Hex value of the Sending CAN ID as a string. Example "7E0"
dstid    |         | Hex value of the Receiving CAN ID as a string. Example "7E8"
data     |         | A string represeting the data bytes to send. Example: "0902" to get VIN

Optional Parameters

Paramter | Default | Description
---------|---------|------------
timeout  | 1500    | The timeout to wait for a response.
maxpkts  |    3    | Stop listening for response packets once this number has been received
### Response Parameters

Returns success if the packet was able to be sent.  When successful a Packets array
will also be set with all of the matching packets.

The listener will exit with success being false if timeout happens before maxpkts are reached.  If
even one packet is recieved the success will be set to true.

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected
Packets  | []      | Contains an array of IDs and DATA of the response packets


