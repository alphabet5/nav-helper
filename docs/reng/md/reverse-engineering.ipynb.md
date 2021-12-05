```python
## Reverse engineering the bluetooth communication for the Kilo5K
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




```python

```
