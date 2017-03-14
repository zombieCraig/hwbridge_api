# Zigbee Extension

This end point is for devices that support Zigbee controls such as Killerbee

## Get supported Killerbee Devices

> Gather supported USB Indexes

```json
{
  "devices": [ '3:35' ]
}
```

```javascript
app.get('/zigbee/supported_devices', function (req, res) {
  //... Gather all capabile zigbee usb IDs and return an array of IDs ...
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ devices: [ '3:35' ] }, null, 1));
})
```

Returns a unique index ID for all supported Zigbee devices.  These IDs are used to 
specify different transceivers if the relay has more than one.

### HTTP Request

`GET http://example.com/api/zigbee/supported_devices`

### Response Parameters

Returns an array of index.

Paramter | Default | Description
---------|---------|------------
devices  | []      | Array of unique ID strings

## Set the Channel

> Sets the Channel

```json
{
  "success": true
}
```

```javascript
app.get('/zigbee/:id/set_channel', function (req, res) {
  var chan = req.params.chan;
  //... Change the channel
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Sets the channel from 11-25

### HTTP Request

`GET http://example.com/api/zigbee/:id/set_channel`

### Query Parameters

Sets the target channel

Paramter | Default | Description
---------|---------|------------
chan     |         | Channel number 11-25

### Response Parameters

Success or failure message

Paramter | Default | Description
---------|---------|------------
success  | false   | Command success

## Inject a raw packet

> Inject a raw packet

```json
{
  "success": true
}
```

```javascript
var URLSafeBase64 = require('urlsafe-base64');
app.get('/zigbee/:id/inject', function (req, res) {
  var data = URLSafeBase64.decode(req.params.data);
  // Inject raw packet (may need special firmware)
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})

```

Injects a raw packet to the current channel.

### HTTP Request

`GET http://example.com/api/zigbee/:id/inject`

### Query Parameters

Sets the target channel

Paramter | Default | Description
---------|---------|------------
data     |         | Raw data in a string to be sent

### Response Parameters

Returns success if the packet was able to be sent.

Paramter | Default | Description
---------|---------|------------
success  | false   | Command success


## Receive a packet

> Receive a packet


```json
{
  "data": <base64 encoded data>
  "valid_crc": <non-zero value if valid>
  "rssi": <rssi value>
}
```

```javascript
var URLSafeBase64 = require('urlsafe-base64');
app.get('/zigbee/:id/recv', function (req, res) {
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ data: URLSafeBase64.encode(data), valid_crc: crc: rssi: rssi }, null, 1));
})
```

Receives a raw packet from the current channel

### HTTP Request

`GET http://example.com/api/zigbee/:id/recv`

### Response Parameters

Paramter | Default | Description
---------|---------|------------
data     | {}      | Base64 encoded raw data
valid_crc  | 0     | Non-zero value if valid
rssi     |         | Signal Strength

## Enable Sniffer Mode

> Enables Receive mode


```json
{
  "success": true
}
```

```javascript
app.get('/zigbee/:id/sniffer_on', function (req, res) {
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Enables receive mode

### HTTP Request

`GET http://example.com/api/zigbee/:id/sniffer_on`

### Response Parameters

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected

## Disable Sniffer Mode

> Disables Receive mode


```json
{
  "success": true
}
```

```javascript
app.get('/zigbee/:id/sniffer_off', function (req, res) {
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Disables sniffer.  Allows for chaning settings such as channel

### HTTP Request

`GET http://example.com/api/zigbee/:id/sniffer_off`

### Response Parameters

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected


