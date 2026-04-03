# HLINK2 Protocol Specification (Draft)

## 1. Introduction

This document describes a JSON-based communication protocol used between air conditioning (AC) units and their Wi-Fi modules over UART. This protocol, referred to here as **HLINK2**, appears to be a successor to the original HLINK binary protocol.

This specification is based entirely on reverse engineering (firmware decompilation and UART traffic analysis). As such, it is incomplete and may contain inaccuracies.

## 2. Conventions and Terminology

The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** in this document are to be interpreted as described in RFC 2119.

* **Wi-Fi module**: The component initiating communication.
* **AC module**: The indoor or outdoor unit responding to requests.
* **Message**: A framed data unit exchanged over UART.

## 3. Protocol Overview

Communication occurs over UART. The Wi-Fi module **MUST** initiate all requests. The AC module **MAY** respond.

Data is exchanged in discrete **messages**, each encapsulated within framing delimiters.

## 4. Message Framing

Each message **MUST** conform to the following structure:

```
\x02{{<header>}{<body>}{<checksum>}}\x03
```

Where:

* `\x02` is the start-of-frame delimiter.
* `\x03` is the end-of-frame delimiter.
* `<header>`, `<body>`, and `<checksum>` are JSON objects without separators.

Note: Although each component (<header>, <body>, <checksum>) uses JSON syntax, the full message is not valid JSON, because it lacks commas between top level components.

## 5. Header

The header **MUST** be a JSON object with the following fields:

```
{"TtIN":1,"MsgN":1,"DataN":1}
```

| Field | Description                         | Observed Values    |
| ----- | ----------------------------------- | ------------------ |
| TtIN  | Unknown purpose                     | 1                  |
| MsgN  | Message sequence number             | 1 (appears unused) |
| DataN | Number of data items in the payload | 0–12               |

## 6. Body

The body **MUST** be a JSON object with the following structure:

```
{"Res":<response_value>,<flag>:{<payload>}}
```

The `Res` field **MUST** be present only in response messages.

### 6.1 Response Field

The `Res` field indicates the status of a response and **MUST** increase the effective data count (`DataN`) by 1.

| Value | Meaning |
| ----- | ------- |
| 0     | VALID   |
| 1     | INVALID |
| 2     | EMPTY   |
| 3     | PARTIAL |

### 6.2 Flag

The flag **MUST** follow the format:

```
<type>_<destination>
```

Example:

```
STS_IDU
```
This flag is used by status messages targeting the indoor unit.

#### 6.2.1 Message Type

| Value | Meaning         |
| ----- | --------------- |
| STS   | Status message  |
| CMD   | Command message |

#### 6.2.2 Destination

| Value | Meaning      |
| ----- | ------------ |
| IDU   | Indoor unit  |
| ODU   | Outdoor unit |

### 6.3 Payload

The payload **MUST** be a mapping of key-value pairs.

#### 6.3.1 Request Messages

* Keys **MUST** be sequential identifiers: `D1`, `D2`, ..., `Dn`.
* Values **MUST** be mnemonic names.

Example:

```
\x02{{"TtIN":1,"MsgN":1,"DataN":12}{"STS_IDU":{"D1":"OnOf","D2":"Mode","D3":"RlMd","D4":"FanS","D5":"FanSw","D6":"SetT","D7":"Tr","D8":"Hr","D9":"Opt1","D10":"Opt2","D11":"Opt3","D12":"Opt4"}}{"ChS":55780}}\x03
```

#### 6.3.2 Response Messages

* Keys **MUST** be mnemonic names.
* Values **MUST** be associated data values (integer, float, or string).

Example:

```
\x02{{"TtIN":1,"MsgN":1,"DataN":13}{"Res":0,"STS_IDU":{"OnOf":0,"Mode":33,"SetT":21.5,"FanS":4,"FanSw":1,"RlMd":33,"Tr":21,"Hr":44,"Opt1":0,"Opt2":0,"Opt3":0,"Opt4":0}}{"ChS":56721}}\x03
```
This message is a valid response to the request defined in Section 6.3.1.

## 7. Checksum

The checksum **MUST** be calculated as:

```
ChS = 0xFFFF - sum(all bytes in <header> and <body>)
```

Example:

```
\x02{{"TtIN":1,"MsgN":1,"DataN":1}{"Res":0,"STS_ODU":{}}{"ChS":63820}}\x03
```

## 8. Mnemonics

Mnemonics are abbreviated identifiers representing device attributes or functions.

A complete list of known mnemonics, along with their inferred meanings and possible values, is provided in a separate file.

## 9. Limitations

This specification is derived from observed behavior and reverse engineering. Implementations **SHOULD** account for undocumented behavior and potential deviations.
