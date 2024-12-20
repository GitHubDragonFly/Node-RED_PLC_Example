# Node-RED PLC Example
Node-RED web browser example of using python to communicate to Allen Bradley PLCs. Deployed on Raspberry Pi 2 with Raspbian Buster OS and with [pylogix](https://github.com/dmroeder/pylogix) and [pycomm3](https://github.com/ottowayi/pycomm3) open source libraries.

This is adapted version of the Node-RED pythonshell [example](https://flows.nodered.org/flow/778859ca2503db35ff0e12341508efef), which made creating this project easy.

Required software for this example (try getting the latest versions):
- [Node-RED](https://nodered.org) and [python3](https://www.python.org)
  - On Debian/Ubuntu systems, you might need to install python3-venv package
- pylogix and pycomm3 will be installed by the flow (internet connection will be required until installed)

Required additional Node-RED packages (use "Manage Palette" option from within Node-RED to install, internet connection required until installed):
- [node-red-dashboard](https://flows.nodered.org/node/node-red-dashboard)
- [node-red-contrib-pythonshell](https://flows.nodered.org/node/node-red-contrib-pythonshell)
- [node-red-contrib-ui-artless-gauge](https://flows.nodered.org/node/node-red-contrib-ui-artless-gauge)

Optionally install any other Node-RED packages (like [ui_level](https://flows.nodered.org/node/node-red-contrib-ui-level) ; [lineargauge](https://flows.nodered.org/node/node-red-node-ui-lineargauge) ; ... etc).

Raspberry Pi 2 was used for this example but any other network enabled hardware, capable of running Node-RED and python3, should work as well. Linux operating systems, like Ubuntu, do have Node-RED package available.

# Node-RED Screenshots

Setup Flow
![Node-RED Dashboard](screenshots/Node-RED%201.png?raw=true)

Dashboard 1
![Node-RED Dashboard](screenshots/Node-RED%202.png?raw=true)

Dashboard 2
![Node-RED Dashboard](screenshots/Node-RED%203.png?raw=true)

# Functionality
- Discover Devices on the network (this is using `pycomm3` library).
- List ControlLogix PLC tags including UDT members (this is using `pycomm3` library).
- Read ControlLogix tags and display their values in the dashboard (this is using `pylogix` library).
- Read SLC500 / MicroLogix tags and display their values in the dashboard (this is using `pycomm3` library).
- Tags can be entered as either of:
  - Single tag, ex: `CT_DINT`
  - Multiple semicolon separated mixed tags, ex: `CT_DINT; CT_REAL; CT_3D_DINTArray[0,3,1]{5}`.
- Reading multiple elements of an array requires the following tag format:
  - `tagName[startIndex]{elementCount}`:
    - `tagName` should be the correct name of the tag as it is in your PLC program, a name like:
      - `CT_DINT` or `CT_REALArray` or `My_INT`
    - `startIndex` is the starting array index, which depending on array dimensions could be either of:
      - `[x]` or `[x,y]` or `[x,y,z]`
    - `elementCount` is the number of consecutive elements to read, for example:
      - `CT_REALArray[0]{15}` - this will request 15 elements starting at `[0]`
      - `CT_DINTArray[0,1,0]{7}` - this will request 7 elements starting at `[0,1,0]`
- The above tag rules are also generally applicable to SLC500 / MicroLogix, ex: `N7:0` or `N7:0{3}`
- Automated or manual process for flow lines with pythonshell nodes
- Node-RED web browser access is generally via IP address of your device + the port, ex: `192.168.1.17:1880`.
- Node-RED Dashboard web browser access is generally via IP address of your device + the port + ui, ex: `192.168.1.17:1880/ui`.
- Debug nodes can be disconnected once you know that everything works.

# Usage
- Install Node-RED, the required nodes and python3:
  - Additionally, python3-venv might also need to be installed
- Copy the `Setup Flow.txt` file's content to the clipboard and import it to Node-RED (just paste it)
- Make sure to read the comments and setup correct values for your IP Address / Processor Slot (Micro800 option is for pylogix only)
- Follow the flow from top to bottom and execute each line by using the inject button:
  - The indicator of success or failure of each step will be in the "Debug" window
  - Exercise patience before proceeding to the next line, especially while pylogix/pycomm3 packages are being installed since it might take a little while.
- Optionally, automate the Device Discovery / Tag Listing by checking the `Inject once after` option of the inject node
- Optionally, automate the Tag Reading by checking the `Inject once after` option (1, 2, 3 seconds respectively top to bottom) of the inject nodes and setting their `Repeat` option to `interval` (3, 3, 3 seconds respectively top to bottom):
  - Try using different intervals until you find the one that works the best for you
- More lines for tag reading can be added by following the existing pattern
- You can also remove whatever you want from the flow (like SLC500 / MicroLogix section if you don't need it)
- You can also convert the flow to only use either SLC500 / MicroLogix or ControlLogix / Micro800
- No special folder was set for this example and all the files / folders were created in the `/home/pi/` folder
  - Any permission issues you might encounter would require that you set correct permissions on the folder you want to use
- After you make any changes to the flow then you will have to click the `Deploy` button

# License
This is all MIT licensed.

# Credits
All the credits go to Node-RED, its contributors and in particular [rodened](https://flows.nodered.org/user/rodened), the author of the original example, for making this an easy work for me.

# Trademarks
Any and all trademarks, either directly or indirectly mentioned in this project, belong to their respective owners.

# Useful Resources
Some Node-RED Library nodes specifically related to PLCs:
- [OpenPLC](https://flows.nodered.org/node/node-red-contrib-openplc)
- [redPlc](https://flows.nodered.org/node/node-red-contrib-redplc)
- [plcindustry](https://flows.nodered.org/node/plcindustry)
- [IO Simulation](https://flows.nodered.org/flow/eb24c4815ed772c244836dbbebd8e9d5)
- [cip-ethernet-ip](https://flows.nodered.org/node/node-red-contrib-cip-ethernet-ip)
  - you can also find it [on GitHub](https://github.com/st-one-io/node-red-contrib-cip-ethernet-ip)
  - some insights from the [AdvancedHMI Forum](https://www.advancedhmi.com/forum/index.php?topic=3268.0) topic
