# nav-helper
 Simple iPhone app for navigation assistance.

## Purpose

This app started out of frustration in identifying remote locations in the wilderness. The Sig Sauer Kilo5K has bluetooth connectivity for sending information on angle and distance of ranged targets. The purpose of this app is to provide missing features that are helpful when navigating in the wilderness.

Intended Features:
- Provide the GPS coordinates of a ranged point allowing a more accurate compass to enter the bearing of the ranged point.
- Optionally use the internal compass to set the bearing using the edge of the device.
- Convert between magnetic and true north using NCEI data.
- Improve accuracy of the ranged point by utilizing the angle and localized high-definition elevation data. (when available)
- Ability to log ranged points in the background, along with current GPS coordinates.
- GPS current position averaging to get a more accurate location.
- Ability to easily convert GPS coordinates between formats.
- Operate when offline.
- Mark current location with a custom icon. Ability to export waypoint to json including icons. Ability to import custom waypoints including icons.

Personal goals include:
- Understanding bluetooth communication.
- Improve my reverse engineering ability for packet data.
- Understand mobile app development ecosystem.
- Documenting my learning process.
- Document an undocumented proprietary protocol.

## Progress

This section is notes on steps I am taking and information that I collect. The current progress and active items are stored in the GitHub project board [here.](https://github.com/alphabet5/nav-helper/projects/1)

- [Capture bluetooth packets.](https://www.bluetooth.com/blog/a-new-way-to-debug-iosbluetooth-applications/)
    - Separate packet captures have been made, one for [pairing](docs/captures/kilo5k-pairing.json) and one for [ranging](docs/captures/kilo5k-range-11.2y-5deg.json). I expect further captures will be necessary to identify how the data is structured for the ranging information.
- Packets were saved from PacketLogger to be viewed in Wireshark, and exported as json.

### Bluetooth Packets

The basic components are not encrypted for the bluetooth communication. Information about bt implementation within swift will be needed before trying to replicate this communication. Further reverse enginering is being postponed until the basic features can be implemented.

Progress decoding the bluetooth communication is stored [here.](docs/reng/md/reverse-engineering.ipynb.md)

iPython notebooks are converted to markdown using nbconvert.

```bash
python3.9 -m jupyter nbconvert --to markdown /yourpath/nav-helper/docs/reng/reverse-engineering.ipynb --output ./md/reverse-engineering.ipynb
```

### Baseline ios app.

- TODO



