---
title: Metasploit Hardware Bridge API

language_tabs:
  - json
  - javascript

toc_footers:
  - <a href='https://www.metasploit.com/'>Metasploit Framework</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - automotive
  - rftransceiver
  - errors

search: true
---

# Introduction API v0.0.2

This is the Metasploit Hardware Bridge API.  The Hardware bridge enables metasploit to interact with physical
hardware in an effort to perform security testing on non-ethernet based systems.  The focus of this documentation
may seem backwards compared to other API documents.  Unlike most API documentation that teaches you how to
interact with the server, this document teaches you how to build in API support into your hardware.  In effect,
guiding you on how to create your own small embedded system that is Metasploit compatible.

We will try to include language bindings for different embedded system languages as well as ruby.  Ruby is used
mainly as a general example and you can view a simple implementation by looking at the Metasploit module:
auxiliary/server/local_hwbridge.

This API documentation page was created with [Slate](https://github.com/tripit/slate).

# Server Setup

You can setup your web server any way you want.  You can write your own REST like client or use
a library.  We will include some examples here using some common libraries.  For Node.js we will
assume the use of the Express library.  Use the 'javascript' tab in the top right to view javascript/nodejs
samples.

> Installing the Express library with nodejs

```javascript
npm init
npm install express --save
```

> Initialize the express REST server as app

```javascript
var http = require('http');
var express = require('express');

var app = express();
```

## Ports & URI

```javascript
// This should come after all of your app routes
app.listen(8080);
```

There is not a standard for what port your device needs to listen to.  The URI to connect can be the root '/' or
anything else.  The auxiliary/client/hwbridge/connect module support a TARGETURI option that can be used to
connect to any URI that is needed for the root of your API server.

## Encryption

```javascript
var fs = require('fs');
var http = require('http');
var https = require('https');
var privateKey  = fs.readFileSync('sslcert/server.key', 'utf8');
var certificate = fs.readFileSync('sslcert/server.crt', 'utf8');

var credentials = {key: privateKey, cert: certificate};
var express = require('express');
var app = express();

// express routes go here

var httpsServer = https.createServer(credentials, app);
```

TLS/SSL Encryption is supported by Metasploit.  If you want to to include support for encrypted communication,
ensure your web server supports or requires TLS or SSL in order to connect.


## Authentication

```javascript
// You can be as sophisticated or as simple as you want
app.use(express.basicAuth(function(user, pass) {
 return user === 'hardcoded' && pass === 'really?!?';
}));
```

Your Hardware device does not have to support authentication.  However, if you want to include it you should
use the basic authentication measure used by web servers.  Use the [RFC 2617](http://www.ietf.org/rfc/rfc2617.txt)

# Get Status and Capabilities

> Requesting Status

```json
{
  "operational": 0,
  "hw_speciality":
    {
      "automtive": true,
      "sdr": false
    },
  "hw_capabilities":
    {
      "can": true,
      "kline": true
    }
}

// Example of last_10_error log
[
  {
    "msg": "Packet collision detected",
    "when": 1482548041
  },
  {
    "msg": "Duplicate message received",
    "when": 1482548095
  }
]

```

```javascript
app.get('/status', function (req, res) {
  //... Gather all the data and return JSON...
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ operational: 1, hw_specialty: { automotive: true }, hw_capabilities: { can: true } }, null, 1));
})
```
> Sample Error log


This endpoint retrieves the status of the hardware bridge.  This should be used when first
connecting to a device to see what the capabilities are.  Each time this endpoint is
queried the hardware should recheck to ensure the devices 

### HTTP Request

`GET http://example.com/api/status`

### Response Parameters

There are only a few required parameters:

* hw_specialty
* hw_capabilities
* api_version

Parameter | Default | Description
--------- | ------- | -----------
operational | 0 | Integer value of HW state: 0 = Unknown, 1 = yes, 2 = no
hw_speciality | {} | What the device specializes in
hw_capabilities | {} | List of specific capabilities the device supports
last_10_errors | [] | Array of the last 10 detected errors
api_version | 0.0.1 | API version device is compliant with
fw_version | 0.0.1 | Firmware version of the device
hw_version | 0.0.1 | Hardware Version
device_name | None | Optional Hardware device name

### hw_specialty

The hardware speciality is a list of major functionality that the hardware device supports. This
list loads base level exentions for metasploit.  For instance, if automotive is set to true then
the automotive extension will automatically be loaded for the session.  Devices can support more
than one specialty.  You do not need to return false for specialties but it could be useful if
the device supports additional specialities but they are currently disabled by the hardware.

Current extensions supported by Metasploit:

Specialties | Description
----------- | -----------
automotive  | Enable the automotive extensions for Metasploit


### hw_capabilties

Specifics of the devices capabilities can be reported through this hash.  For instance if you have
a device that supports different types of buses or hardware interfaces they can be returned through
this method.  These capabilities are used by modules to determine which types are supported on this
device.

Some capabilities are have special meaning:

Special Capabilities | Description
-------------------- | -----------
custom_methods       | If set to true, custom_methods will be called to auto-populate the list of custom hardware controls accessible in Metasploit

### last_10_errors

The device can report errors that were detected.  These errors could be hardware errors or
communication errors.

# Gathering Statistics

> Gather basic usage statistics

```json
{
  "uptime":  3902,
  "packet_stats":  984,
  "last_request": 1482867802,
  "voltage": 11.8
}
```

```javascript
app.get('/statistics', function (req, res) {
  // Gather stats to report in JSON
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ /* Current Stats */ }, null, 1));
})
```

Returns some basic information on the current devices runtime operations.

### HTTP Request

`GET http://example.com/api/statistics`

### Response Parameters

All responses are optional.

Paramter | Default | Description
---------|---------|------------
uptime   |  0      | Seconds of uptime
packet_stats | 0   | Packets sent through device
last_request | 0   | Epoch of when the last packet was sent
voltage | 0 | Current voltage usage

# Device Settings

This endpoint is for adjusting settings on the hardware device.  This endpoint is optional.

## Get Datetime

> Get the Date and Time of the device

```json
{
  "system_datetime": 1482866664
}
```

```javascript
app.get('/settings/datetime', function (req, res) {
  var epoch = new Date() / 1000;
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ system_datetime: epoch }, null, 1));
})
```

Gets the datetime stamp of the remote hardware

### HTTP Request

`GET http://example.com/api/settings/datetime`

### Response Parameters

Paramter | Default | Description
---------|---------|------------
system_datetime   |  0      | Epoch

## Get Timezone

> Returns the current timezone

```json
{
  "system_timezone": "PDT"
}
```

```javascript
app.get('/settings/timezone', function (req, res) {
  var timezone = new Date().toString().match(/\(([A-Za-z\s].*)\)/)[1]
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ system_timezone: timezone }, null, 1));
})
```

Get the timezone reported by the hardware

### HTTP Request

`GET http://example.com/api/settings/timezone`

### Response Parameters

Paramter | Default | Description
---------|---------|------------
system_timezone   |  ""      | String response such as EST, PDT, etc.

## Get IP Configuration

> Get the IP Configuration Info

```json
{
  "setup": "dynamic",
  "ip4_addr: "172.16.17.18",
  "ip4_netmask": 255.255.0.0",
  "ip4_gw" : "172.16.0.1",
  "ip6_addr: "fe80::73a7:4cee:f4ac:b7e3",
  "ip6_netmask": "/64",
  "dns": [
    "8.8.8.8",
    "8.8.4.4"
  ]
}
```

```javascript
app.get('/settings/ip/config', function (req, res) {
  // Gather and report IP information
});
```

Get the remote devices IP settings

### HTTP Request

`GET http://example.com/api/settings/ip/config`

### Response Parameters

Paramter | Default | Description
---------|---------|------------
setup    |  ""     | Reports if the IP was statically or dynamically setup
ip4_addr | "172.0.0.1" | The current ip4 IP address of the HW device
ip4_netmask| "255.255.255.0" | IP 4 netmask
ip4_gw   | "" | IP 4 Gateway
ip6_addr | "::1"   | The current ip6 IP address of the HW device
ip6_netmask | "" | The IP6 Netmask
ip6_gw   | "" | IP6 gateway
dns      | []      | Array of dns addresses defined by "addr:"

# Low-level Firmware Control

This endpoint is for low level hardware controls.  Typically used for troubleshooting or
remote updates.  This endpoint is optional.

## Reboot Device

```json
{
  "status": "Rebooting"
}
```

```javascript
var exec = require('child_process').exec;

function execute(command){
    exec(command, function(error, stdout, stderr));
}

app.get('/control/reboot', function (req, res) {
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ status: "Rebooting" }, null, 1));
  // You would need the account to be in sudoers file w/o a need for a password
  execute('sudo /sbin/reboot');
});
```

Reboots the remote device.

### HTTP Request
 
`GET http://example.com/api/control/reboot`

### Response Parameters

Paramter | Default | Description
---------|---------|------------
status   | "Not Supported" | Reports "Rebooting" if control is supported

## Factory Reset

```json
{
  "status": "Resetting"
}
```

```javascript
app.get('/control/factory_reset', function (req, res) {
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ status: "Resetting" }, null, 1));
  // Execute whatever fallback firmware control is supported
});
```

Resets the device back to its initial settings if supported.  This could be used in
conjunction with other hardware controls such as a reset button.

### HTTP Request

`GET http://example.com/api/control/factory_reset`

### Response Parameters

Paramter | Default | Description
---------|---------|------------
status   | "Not Supported" | Reports "Resetting" if control is supported

# Custom Hardware Methods

Hardware can support functionality that is not originally known to the Metasploit
framework.  These methods will automatically show up in the hwbridge interface.

## Get a list of methods

It is possible to include custom hardware controls by sending method prototypes
to the Metasploit framework.

<aside class=notice>You will need to set custom_methods to true when reporting hw_capabilities
in order to have Metasploit auto-populated the command listings</aside>

```json
{
  "Methods": [
    {
      "method_name": "rewrite_protected_memory",
      "method_desc": "Overwrites protected memory with specified RFID",
      "args": [
        {
          "arg_name": "RFID",
          "arg_type": "Int",
          "required": true
        }
      ],
      "return": "string"
    }
  ]
}
```

```javascript
// To get a list of custom methods
app.get('/custom_methods', function (req, res) {
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ Methods: [ /* Custom definitions */ ] }, null, 1));
});
```

### HTTP Request

`GET http://example.com/api/custom_methods`

### Response Parameters

The response JSON defines all attributes of the custom methods.  See JSON
example for more clarificaiton.

Paramter | Default | Description
---------|---------|------------
Methods  |   []    | Array of method hashes
method_name |      | Unique utf8 name that will show up as a command
method_desc |      | A brief description for what the command does
args     |   []    | Array of argument definitions
arg_name |         | Unique argument name that will be passed as a URI argument
arg_type |         | Type defines desired type.  Validity checking should still be done
required |  false  | Is this argument required?
return   |  "nil"  | How to format the return value

Returned values are return as a hash with key of "value".  The defined return type indicates
how the value should be formated and displayed to the end user.  Options are:

Return Types | Description
------------ | -----------
nil          | No return expected
int          | Integer value
hex          | Integer value will be convert to a hex and represented with 0x00 syntax
boolean      | Boolean true or false
float        | Floating point number
string       | String will be directly returned

If there is an error you may return "success" as false as well.  If you set a return
value for "status" the result will be printed to the command line.

## Calling a custom method

```json
{
  "value": "Memory updated"
}
```

```javascript
// If you had a custom method called "custom/rewrite_protected_memory"
app.get('/custom/rewrite_protected_meory', function (req, res) {
  // if you have an arg_name defined as "rfid"
  var rfid = req.query.rfid;
  // Do you custom work...
  res.setHeader('Content-Type', 'application/json');
  res.send(JSON.stringify({ value: "Great success, updated memory" }, null, 1));
});
```

### HTTP Request

The HTTP Request is always a GET request.  These are custom URIs that are supported by the hardware
and reported via calls to the custom_methods endpoint.  For instance, custom_methods defined
a method_name of "rewrite_protected_memory"  Then the call would be directly off the root:

`GET http://example.com/api/rewrite_protected_memory`

However, if the method_name was "custom/rewrite_protected_memory" then the command via Metasploit
would still be "rewrite_protected_memory" however the URI would be:

`GET http://example.com/api/custom/rewrite_protected_memory`

### Request Paramters

Parameters are defined by the methods "args" attribute.  This is an array of possible parameter
values.  The value types are defined by the "arg_type" definitions however, Metasploit only performs
basic validation and the values should still be validated by the hardware.  The "required" attribute
denotes if the argument is required before the full URI will be created.

### Response Parameters

All responses should have a "value".  If the "return" type is set to "nil" then an empty hash
can be returned instead.  Typically you will want to use the "string" return type as it provides
the most control.  However, you can use the other types and have Metaplsoit do some formating for
you.


