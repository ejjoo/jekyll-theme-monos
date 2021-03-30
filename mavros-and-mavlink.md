---
description: They are bridge and communication protocol between FC < - > GCS
---

# MAVROS & MAVLink

## Install MAVROS



## What is MAVROS?



## What is MAVLink?

MAVLink is a protocol for communicating with small unmanned vehicle.

MAVLink protocol ~~enable the communication~~ between GCS\(Ground Control Station\) and PX4 



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

* The minimum packet length is 8 bytes for acknowledgment packets without payload.
* The maximum packet length is 263 bytes for full payload.



#### MAVLink v2 protocol packet structure

![Source : https://mavlink.io/en/guide/serialization.html](.gitbook/assets/image%20%289%29.png)

<table>
  <thead>
    <tr>
      <th style="text-align:left"><del>Field name</del>
      </th>
      <th style="text-align:left"><del>Index - Bytes</del>
      </th>
      <th style="text-align:left"><del>Purpose and function</del>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><del>header1(Start-of-frame)</del>
      </td>
      <td style="text-align:left"><del>0</del>
      </td>
      <td style="text-align:left"><del>Header pointing to the starting point(0xFE)</del>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><del>header2(Payload-length)</del>
      </td>
      <td style="text-align:left"><del>1</del>
      </td>
      <td style="text-align:left"><del>value of Payload-length (n)</del>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><del>header3(Packet sequence)</del>
      </td>
      <td style="text-align:left"><del>2</del>
      </td>
      <td style="text-align:left">
        <p><del><br /></del>
        </p>
        <p><del>order value in the total packet</del>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><del>header4(System ID)</del>
      </td>
      <td style="text-align:left"><del>3</del>
      </td>
      <td style="text-align:left"><del>ID of the SENDING System</del>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><del>header5(Component ID)</del>
      </td>
      <td style="text-align:left"><del>4</del>
      </td>
      <td style="text-align:left"><del>ID of the SENDING Component</del>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><del>header6(Message ID)</del>
      </td>
      <td style="text-align:left"><del>5</del>
      </td>
      <td style="text-align:left"><del>ID of the message : definition of Payload</del>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><del>data(Payload)</del>
      </td>
      <td style="text-align:left"><del>6 (n+6)</del>
      </td>
      <td style="text-align:left"><del>Data of the message, depends on the message id</del>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><del>checksum1 CRC</del>
      </td>
      <td style="text-align:left"><del>(n+7)</del>
      </td>
      <td style="text-align:left"><del>Message integrity check</del>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><del>checksum2 CRC</del>
      </td>
      <td style="text-align:left"><del>(n+8)</del>
      </td>
      <td style="text-align:left"><del>Network integrity check</del>
      </td>
    </tr>
  </tbody>
</table>

-&gt; this table need to be edited. for v2

* The minimum packet length is 12 bytes for acknowledgment packets without payload.
* The maximum packet length is 280 bytes for a signed message that uses the whole payload.



### About Telemetry radio





