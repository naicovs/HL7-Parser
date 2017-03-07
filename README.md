[![Build Status](https://travis-ci.org/mrferos/HL7-Parser.png)](https://travis-ci.org/mrferos/HL7-Parser)

# HL7 API: README


## Contents

1.0 Introduction<br/>
2.0 Usage<br/>
2.1 Creating messages<br/>
2.2 Sending/receiving<br/>


## 1.0 Introduction


This is the PHP HL7 API. It is a very simple, but rather flexible API
for use in PHP applications. The API is a word for word translation of
the Perl Net-HL7 package, based on version 0.72.

The focus of this API is on providing functionality for the
composition, manipulation and decomposition of HL7 messages, not
providing hundreds of prefab HL7 messages. In HL7 terms: this API
focuses on the HL7 syntax, not the semantics. This makes the API
support all versions that use the classic HL7 syntax, that is,
versions untill 3.x. This API does not do XML!


## 2.0 Usage

### 2.1 Creating messages

To create an HL7 message, simply do this:

```
$msg = new HL7\Message();
```

this gives you an empty message, that doesn't containt ANY segments
(not even the header segment). You might also have a string
representation of your message at hand, in which case you can do this:

```
$msg = new HL7\Message(<your string>);
```

Make sure to escape any characters that have special meaning in PHP!


### 2.2 Sending/receiving

A full example of sending and receiving a message is listed here,
assuming you have an HL7 server on your localhost, listening to port
12002 (you might want to use the hl7d from the Perl HL7 Toolkit
package as a simple test server; http://hl7toolkit.sourceforge.net/):


```php
<?php
require 'vendor/autoload.php';

$msg  = new HL7\Message();
$msg->addSegment(new HL7\Segments\MSH());

$seg1 = new HL7\Segment("PID");

$seg1->setField(3, "XXX");

$msg->addSegment($seg1);

echo "Trying to connect";
$socket = new Net_Socket();
$success = $socket->connect("localhost", 12002);

if ($success instanceof PEAR_Error) {
    echo "Error: {$success->getMessage()}";
    exit(-1);
}

// this version of Connection doesn't contain $socket->connect
$conn = new HL7\Connection($socket);

echo "Sending message\n" . $msg->toString(true);

$resp = $conn->send($msg);

$resp || exit(-1);

echo "Received answer\n" . $resp->toString(true);

$conn->close();
?>
```

To use composer add this repository to the composer.json 
file and select dev-master in the require statement.
Example:
```
{
    "repositories": [
        {
            "type": "vcs",
            "url": "https://github.com/naicovs/hl7-parser"
        }
    ],
    "require": {
        "mrferos/hl7-parser": "dev-master",
        "pear/pear": "dev-master"
        "pear/net_socket": "^1.0"
    }
}
```
