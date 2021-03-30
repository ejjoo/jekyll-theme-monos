---
description: They are bridge and communication protocol between FC < - > GCS
---

# MAVROS & MAVLink

## Install MAVROS



## What is MAVROS?



## What is MAVLink?

MAVLink is a protocol for communicating with small unmanned vehicle.

###  Packet

#### MAVLink protocol packet structure

![https://ko.wikipedia.org/wiki/MAVLink](.gitbook/assets/image%20%2811%29.png)

MAVLink Frame consists of 6headers, 2checksum and one field with data.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Field name</th>
      <th style="text-align:left">Index - Bytes</th>
      <th style="text-align:left">Purpose and function</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">header1(Start-of-frame)</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">Header pointing to the starting point(0xFE)</td>
    </tr>
    <tr>
      <td style="text-align:left">header2(Payload-length)</td>
      <td style="text-align:left">1</td>
      <td style="text-align:left">value of Payload-length (n)</td>
    </tr>
    <tr>
      <td style="text-align:left">header3(Packet sequence)</td>
      <td style="text-align:left">2</td>
      <td style="text-align:left">
        <p>
          <br />
        </p>
        <p>order value in the total packet</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">header4(System ID)</td>
      <td style="text-align:left">3</td>
      <td style="text-align:left">ID of the SENDING System</td>
    </tr>
    <tr>
      <td style="text-align:left">header5(Component ID)</td>
      <td style="text-align:left">4</td>
      <td style="text-align:left">ID of the SENDING Component</td>
    </tr>
    <tr>
      <td style="text-align:left">header6(Message ID)</td>
      <td style="text-align:left">5</td>
      <td style="text-align:left">ID of the message : definition of Payload</td>
    </tr>
    <tr>
      <td style="text-align:left">data(Payload)</td>
      <td style="text-align:left">6 (n+6)</td>
      <td style="text-align:left">Data of the message, depends on the message id</td>
    </tr>
    <tr>
      <td style="text-align:left">checksum1 CRC</td>
      <td style="text-align:left">(n+7)</td>
      <td style="text-align:left">Message integrity check</td>
    </tr>
    <tr>
      <td style="text-align:left">checksum2 CRC</td>
      <td style="text-align:left">(n+8)</td>
      <td style="text-align:left">Network integrity check</td>
    </tr>
  </tbody>
</table>

### 

### About Telemetry radio





