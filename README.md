# nav-helper
 Simple iphone app for navigation assistance.

This documentation is generated from docs/reng/reverse-engineering.ipynb using nbconvert.

```bash
python3.9 -m jupyter nbconvert --to markdown ./docs/reng/reverse-engineering.ipynb --output ../../README.md
```

## Purpose

This app started out of frustration in identifying remote locations in the wilderness. The Sig Sauer Kilo5K has bluetooth connectivity for sending information on angle and distance of ranged targets. The purpose of this app is to provide missing features that are helpful when navigating in the wilderness.

Personal goals include:
- Understanding bluetooth communication.
- Improve my reverse engineering ability for packet data.
- Understand mobile app development ecosystem.
- Documenting my learning process.
- Document an undocumented proprietary protocol.

Intended Features:
- Provide the GPS coordinates of a ranged point allowing a more accurate compass to enter the bearing of the ranged point.
- Optionally use the internal compass to set the bearing using the edge of the device.
- Convert between magnetic and true north using NCEI data.
- Improve accuracy of the ranged point by utilizing the angle and localized high-definition elevation data. (when available)
- Ability to log ranged points in the background, along with current GPS coordinates.
- GPS current position averaging to get a more accurate location.
- Ability to easily convert GPS coordinates between formats.
- Operate when offline.

## Progress

This section is notes on steps I am taking and information that I collect.

- [Capture bluetooth packets.](https://www.bluetooth.com/blog/a-new-way-to-debug-iosbluetooth-applications/)
    - Separate packet captures have been made, one for [pairing](docs/captures/kilo5k-pairing.json) and one for [ranging](docs/captures/kilo5k-range-11.2y-5deg.json). I expect further captures will be necessary to identify how the data is structured for the ranging information.
- Packets were saved from PacketLogger to be viewed in Wiresshark.

### Bluetooth Packets
RF = Range Finder
P = Phone

Ranging 11.2y 5 deg
1. RF->P MTU Request (Rx MTU: 244)
2. P->RF MTU Response (Rx MTU: 244)
3. P->RF L2CAP (???)
4. P->RF Read By Type GATT Characteristic Declaration
5. RF->P Rcvd Error Response Attribute not found (This is consistant for every range)
6. P->RF Write Request Handle: 0x0010 Value: 0100



```python
## Reverse engineering the bluetooth communication for the Kilo5K
```


```python
import json
from rich import traceback
from rich import print

traceback.install()

packets = json.load(open('../captures/kilo5k-range-11.2y-5deg.json', 'r'))


def parse(input_hex):
    input_list = bytes.fromhex(input_hex).decode('ASCII').split(',')
    desc = {
            0: 'Unknown0',
            1: 'Unknown1',
            2: 'Unknown2',
            3: 'Unknown3',
            4: 'Unknown4',
            5: 'Range(y)5',
            6: 'Unknown6',
            7: 'Unknown7',
            8: 'Bearing?8',
            9: 'Unknown9',
            10: 'Unknown10',
            11: 'Angle(up or down?)',
            12: 'End?'
           }
    vals = dict()
    print(len(input_list))
    for i in range(len(input_list)):
        vals[desc[i]] = input_list[i]
    return vals

```


```python
data = parse(packets[-2]['_source']['layers']['btatt']['btatt.value'].replace(':',''))
print(data)
```


<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #008080; text-decoration-color: #008080; font-weight: bold">13</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="font-weight: bold">{</span>
    <span style="color: #008000; text-decoration-color: #008000">'Unknown0'</span>: <span style="color: #008000; text-decoration-color: #008000">':AB'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'Unknown1'</span>: <span style="color: #008000; text-decoration-color: #008000">'2'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'Unknown2'</span>: <span style="color: #008000; text-decoration-color: #008000">'0.000'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'Unknown3'</span>: <span style="color: #008000; text-decoration-color: #008000">'0.000'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'Unknown4'</span>: <span style="color: #008000; text-decoration-color: #008000">'0.000'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'Range(y)5'</span>: <span style="color: #008000; text-decoration-color: #008000">'11.200'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'Unknown6'</span>: <span style="color: #008000; text-decoration-color: #008000">'0.000'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'Unknown7'</span>: <span style="color: #008000; text-decoration-color: #008000">'0.000'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'Bearing?8'</span>: <span style="color: #008000; text-decoration-color: #008000">'270.000'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'Unknown9'</span>: <span style="color: #008000; text-decoration-color: #008000">'0.000'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'Unknown10'</span>: <span style="color: #008000; text-decoration-color: #008000">'0.000'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'Angle(up or down?)'</span>: <span style="color: #008000; text-decoration-color: #008000">'4.564'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'End?'</span>: <span style="color: #008000; text-decoration-color: #008000">'B5\r'</span>
<span style="font-weight: bold">}</span>
</pre>




```python
print(bytes.fromhex(packets[-2]['_source']['layers']['btatt']['btatt.value'].replace(':','')).decode('ASCII').split(','))
```


<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="font-weight: bold">[</span>
    <span style="color: #008000; text-decoration-color: #008000">':AB'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'2'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'0.000'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'0.000'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'0.000'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'11.200'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'0.000'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'0.000'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'270.000'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'0.000'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'0.000'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'4.564'</span>,
    <span style="color: #008000; text-decoration-color: #008000">'B5\r'</span>
<span style="font-weight: bold">]</span>
</pre>


