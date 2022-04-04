---
description: They are bridge and communication protocol between FC < - > GCS
---

# MAVROS & MAVLink

## Install MAVROS



## What is MAVROS?

MAVROS is a project that develops a node operating in ROS using MAVLink protocol.

MAVROS can be installed in Onboard\_System, be responsible for communication between FC &lt;-&gt; Onboard &lt;-&gt; GroundStation



## What is MAVLink?

MAVLink is a protocol for communicating with small unmanned vehicle.

MAVLink protocol enable the communication between GCS\(Ground Control Station\) and PX4 



###  Packet

#### MAVLink v1 protocol packet structure

![Source : https://mavlink.io/en/guide/serialization.html](.gitbook/assets/image%20%281%29.png)

MAVLink v1 Frame consists of 6headers, 2checksum and one field with data.

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
      <td style="text-align:left">Payload-length (n)</td>
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

* The minimum packet length is 8 bytes for acknowledgment packets without payload.
* The maximum packet length is 263 bytes for full payload.



#### MAVLink v2 protocol packet structure

![Source : https://mavlink.io/en/guide/serialization.html](.gitbook/assets/image%20%289%29.png)

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
      <td style="text-align:left">Payload-length (n)</td>
    </tr>
    <tr>
      <td style="text-align:left">header3(INC Flags)</td>
      <td style="text-align:left">2</td>
      <td style="text-align:left">
        <p>
          <br />
        </p>
        <p>Incompatibility Flags</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">header4(CMP Flags)</td>
      <td style="text-align:left">3</td>
      <td style="text-align:left">Compatibility Flags</td>
    </tr>
    <tr>
      <td style="text-align:left">header5(SEQ number)</td>
      <td style="text-align:left">4</td>
      <td style="text-align:left">Packet sequence number</td>
    </tr>
    <tr>
      <td style="text-align:left">header6(System ID)</td>
      <td style="text-align:left">5</td>
      <td style="text-align:left">ID of system</td>
    </tr>
    <tr>
      <td style="text-align:left">header7(Component ID)</td>
      <td style="text-align:left">6</td>
      <td style="text-align:left">ID of component</td>
    </tr>
    <tr>
      <td style="text-align:left">header8(Msg ID)</td>
      <td style="text-align:left">7 to 9</td>
      <td style="text-align:left">ID of message type in payload</td>
    </tr>
    <tr>
      <td style="text-align:left">data(Payload)</td>
      <td style="text-align:left">10(n+10)</td>
      <td style="text-align:left">Message data(depends on mesage type)</td>
    </tr>
    <tr>
      <td style="text-align:left">checksum CRC</td>
      <td style="text-align:left">(n+11)</td>
      <td style="text-align:left">CRC-16 / MCRF4XX for msg</td>
    </tr>
    <tr>
      <td style="text-align:left">signature</td>
      <td style="text-align:left">(n+12) to (n+25)</td>
      <td style="text-align:left">(optional) signature to ensure the link is tamper-proof</td>
    </tr>
  </tbody>
</table>

* The minimum packet length is 12 bytes for acknowledgment packets without payload.
* The maximum packet length is 280 bytes for a signed message that uses the whole payload.



### About Telemetry radio





