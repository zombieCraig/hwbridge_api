# RF Transceiver Extension

This end point is for devices that support the rftransciiver hw_specialty.

## Get supported USB Indexes

> Gather supported USB Indexes

```json
{
  "indexes": [ 0 ]
}
```

```javascript
app.get('/rftransceiver/supported_idx', function (req, res) {
  //... Gather all capabile buses and return an array of bus_names ...
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ indexes: [ 0 ] }, null, 1));
})
```

Returns a unique index ID for all supported USB indexes.  These IDs are used to 
specify different transceivers if the relay has more than one.

### HTTP Request

`GET http://example.com/api/rftransceiver/supported_idx`

### Query Parameters

Required Parameters

Paramter | Default | Description
---------|---------|------------
idx      | 0       | Index number
freq     | 0       | Frequency, Example: 433000000

Optional Parameters

Paramter | Default | Description
---------|---------|------------
mhz      | 24      | Mhz

### Response Parameters

Returns an array of index.

Paramter | Default | Description
---------|---------|------------
indexes  | []      | Array of unique ID numbers

## Set the Frequency

> Sets the Frequency

```json
{
  "success": true
}
```

```javascript
app.get('/rftransceiver/:idx/set_freq', function (req, res) {
  var freq = req.params.freq;
  //... Report the current bus configuration ...
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Sets the frequency

### HTTP Request

`GET http://example.com/api/rftransceiver/:idx/set_freq`

### Response Parameters

Success or failure message

Paramter | Default | Description
---------|---------|------------
success  | false   | Command success

## Get supported Modulations

> Gets a list of supported modulations

```json
[
  ["2FSK", "GFSK", "ASK/OOK", "MSK", "2FSK/Manchester", "GFSK/Manchester", "ASK/OOK/Manchester", "MSK/Manchester"]
]
```

```javascript
app.get('/rftransciever/:idx/get_modulations', function (req, res) {
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify(["2FSK", "GFSK", "ASK/OOK"], null, 1));
})

```

Returns a list of all supported modulations.  These strings are the keyword used to set a modulation.

### HTTP Request

`GET http://example.com/api/rftransceiver/:idx/get_modulations`

### Response Parameters

Returns success if the packet was able to be sent.

Paramter | Default | Description
---------|---------|------------
         | []      | List of string representation of modulation settings

## Set Modulation

> Setup a modulation


```json
{
  "success": true
}
```

```javascript
app.get('/rftransceiver/:idx/set_modulation', function (req, res) {
  var mod = req.params.mod;
  //... Set the modulation
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Sets a desired modulation technique.  See get_modulations for a list of supported modulations

### HTTP Request

`GET http://example.com/api/reftransciever/:idx/set_modulation`

### Query Parameters

Required Parameters

Paramter | Default | Description
---------|---------|------------
mod      | ""      | Modulation name.  Example ASK/OOK

### Response Parameters

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected

## Set Packet's Fixed Length

> Set fixed length


```json
{
  "success": true
}
```

```javascript
app.get('/rftransceiver/:idx/make_packet_flen', function (req, res) {
  var flen = req.params.len;
  //... Set fixed length
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Sets the fixed length value of a packet

### HTTP Request

`GET http://example.com/api/reftransciever/:idx/make_packet_flen`

### Query Parameters

Required Parameters

Paramter | Default | Description
---------|---------|------------
len      | 0       | Fixed length

### Response Parameters

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected

## Set Packet's Variable Length

> Set variable length


```json
{
  "success": true
}
```

```javascript
app.get('/rftransceiver/:idx/make_packet_vlen', function (req, res) {
  var vlen = req.params.len;
  //... Set variable length
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Sets the variable length value of a packet

### HTTP Request

`GET http://example.com/api/reftransciever/:idx/make_packet_vlen`

### Query Parameters

Required Parameters

Paramter | Default | Description
---------|---------|------------
len      | 0       | Fixed length

### Response Parameters

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected

## Set Mode

> Set mode TX, RX or IDLE


```json
{
  "success": true
}
```

```javascript
app.get('/rftransceiver/:idx/set_mode', function (req, res) {
  var mode = req.params.mode;
  //... Set the mode
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Sets the transmission mode.  Options are TX, RX or IDLE

### HTTP Request

`GET http://example.com/api/reftransciever/:idx/set_mode`

### Query Parameters

Required Parameters

Paramter | Default | Description
---------|---------|------------
mode     | 0       | Mode: TX, RX or IDLE

### Response Parameters

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected

## Enable Packet CRC

> Enable CRC


```json
{
  "success": true
}
```

```javascript
app.get('/rftransceiver/:idx/enable_packet_crc', function (req, res) {
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Sets the device to generate a CRC

### HTTP Request

`GET http://example.com/api/reftransciever/:idx/enable_packet_crc`

### Response Parameters

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected

## Enable Manchester Encoding

> Enable manchester encoding


```json
{
  "success": true
}
```

```javascript
app.get('/rftransceiver/:idx/enable_manchester', function (req, res) {
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Enables Manchester encoding

### HTTP Request

`GET http://example.com/api/reftransciever/:idx/enable_manchester`

### Response Parameters

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected

## Set Channel

> Set channel


```json
{
  "success": true
}
```

```javascript
app.get('/rftransceiver/:idx/set_channel', function (req, res) {
  var channel = req.params.channel;
  //... Set the channel
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Sets the channel

### HTTP Request

`GET http://example.com/api/reftransciever/:idx/set_channel`

### Query Parameters

Required Parameters

Paramter | Default | Description
---------|---------|------------
channel  | 0       | Channel number

### Response Parameters

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected

## Set Channel Bandwidth

> Set channel's bandwidth


```json
{
  "success": true
}
```

```javascript
app.get('/rftransceiver/:idx/set_channel_bandwidth', function (req, res) {
  var bw = req.params.bw;
  //... Set the channel's bandwidth
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Sets the channel bandwidth

### HTTP Request

`GET http://example.com/api/reftransciever/:idx/set_channel_bandwidth`

### Query Parameters

Required Parameters

Paramter | Default | Description
---------|---------|------------
bw       | 0       | Channel bandwidth

Optional Parameters

Paramter | Default | Description
---------|---------|------------
mhz      | 24      | Mhz

### Response Parameters

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected

## Set Channel SPC

> Set channel's spc


```json
{
  "success": true
}
```

```javascript
app.get('/rftransceiver/:idx/set_channel_spc', function (req, res) {
  // All params are optional
  var chanspc = req.params.chanpsc;
  var chanspc_m = req.params.chanpsc_m;
  var chanspc_e = req.params.chanpsc_e;
  var mhz = req.params.mhz;
  //... Set the channel's spc
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Sets the channel spc

### HTTP Request

`GET http://example.com/api/reftransciever/:idx/set_channel_spc`

### Query Parameters

Optional Parameters

Paramter | Default | Description
---------|---------|------------
chanspc  | 0       | Main chanspc
chanspc_m | 0      | If you don't set chanspc you can set both _m and _e
chanspc_e | 0      | If you don't set chanspc you can set both _m and _e
mhz      | 24      | Mhz

### Response Parameters

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected

## Set Baud Rate

> Set baud rate


```json
{
  "success": true
}
```

```javascript
app.get('/rftransceiver/:idx/set_baud_rate', function (req, res) {
  var rate = req.params.rate;
  //... Set the baud rate
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Sets the baud rate

### HTTP Request

`GET http://example.com/api/reftransciever/:idx/set_baud_rate`

### Query Parameters

Required Parameters

Paramter | Default | Description
---------|---------|------------
rate     | 0       | baud rate. Example: 2400

Optional Parameters

Paramter | Default | Description
---------|---------|------------
mhz      | 24      | Mhz

### Response Parameters

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected

## Set Deviation

> Set deviation


```json
{
  "success": true
}
```

```javascript
app.get('/rftransceiver/:idx/set_deviation', function (req, res) {
  var deviat = req.params.deviat;
  //... Set the deviation
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Sets the deviation

### HTTP Request

`GET http://example.com/api/reftransciever/:idx/set_deviation`

### Query Parameters

Required Parameters

Paramter | Default | Description
---------|---------|------------
deviat   | 0       | Deviat value

Optional Parameters

Paramter | Default | Description
---------|---------|------------
mhz      | 24      | Mhz

### Response Parameters

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected

## Set Sync Word

> Set sync word


```json
{
  "success": true
}
```

```javascript
app.get('/rftransceiver/:idx/set_sync_word', function (req, res) {
  var word = req.params.word;
  //... Set the sync word
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Sets the sync word.

### HTTP Request

`GET http://example.com/api/reftransciever/:idx/set_sync_word`

### Query Parameters

Required Parameters

Paramter | Default | Description
---------|---------|------------
word     | 0       | Sync Word

### Response Parameters

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected

## Set Sync Mode

> Set sync Mode


```json
{
  "success": true
}
```

```javascript
app.get('/rftransceiver/:idx/set_sync_mode', function (req, res) {
  var mode = req.params.mode;
  //... Set the sync mode
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Sets the sync mode. Sets the accuracy of the sync

### HTTP Request

`GET http://example.com/api/reftransciever/:idx/set_sync_mode`

### Query Parameters

Required Parameters

Paramter | Default | Description
---------|---------|------------
mode     | 0       | Sync Mode

### Response Parameters

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected

## Set Max Power

> Set the power to max


```json
{
  "success": true
}
```

```javascript
app.get('/rftransceiver/:idx/set_maxpower', function (req, res) {
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Sets the maximum power level.  Frequency should be set first

### HTTP Request

`GET http://example.com/api/reftransciever/:idx/set_maxpwer`

### Response Parameters

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected

## Set Power Level

> Set the power level

```json
{
  "success": true
}
```

```javascript
app.get('/rftransceiver/:idx/set_power', function (req, res) {
  var power = req.params.power;
  //... Set the power level
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Sets the power level.  Power can vary depending on frequency

### HTTP Request

`GET http://example.com/api/reftransciever/:idx/set_power`

### Query Parameters

Required Parameters

Paramter | Default | Description
---------|---------|------------
power    | 0       | Power level

### Response Parameters

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected

## Transmit a packet

> Send a packet


```json
{
  "success": true
}
```

```javascript
app.get('/rftransceiver/:idx/rfxmit', function (req, res) {
  var data = req.params.data;
  // Optional
  var repeat = req.params.repeat;
  var offset = req.params.offset;
  //... Transmit the packet
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Transmits the packet.  Optionally you can set the repeat amount and the data offset.
The data is Base64 encoded and can contain binary information

### HTTP Request

`GET http://example.com/api/reftransciever/:idx/rfxmit`

### Query Parameters

Required Parameters

Paramter | Default | Description
---------|---------|------------
data     | 0       | Base64 Encoded binary blob

Optional Parameters

Paramter | Default | Description
---------|---------|------------
repeat   | 0       | Repeat transmition X times
offset   | 0       | Data offset to use

### Response Parameters

Paramter | Default | Description
---------|---------|------------
success  | false   | Returns true of no errors were detected

## Receive a packet

> Recieves packets

```json
{
  "success": true
}
```

```javascript
app.get('/rftransceiver/:idx/rfrecv', function (req, res) {
  // Optional
  var timeout = req.params.timeout;
  var blocksize = req.params.blocksize;
  //... Transmit the packet
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ success: true }, null, 1));
})
```

Recieves a packet.  Packets are sent over Base64 encoded and
can contain binary data.  A timestamp is also included

### HTTP Request

`GET http://example.com/api/reftransciever/:idx/rfxmit`

### Query Parameters

Optional Parameters

Paramter | Default | Description
---------|---------|------------
timeout  | 0       | Timeout value
blocksize| 0       | Blocksize

### Response Parameters

Paramter | Default | Description
---------|---------|------------
data     | ""      | Base64 Encoded data
ts       | 0       | Timestamp

