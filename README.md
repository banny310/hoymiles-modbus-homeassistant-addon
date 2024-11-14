
# Hoymiles Modbus DTU Solar Data Gateway Add-on

An application to read Hoymiles Gateway Solar Data using direct communication with Hoymiles DTU over Modbus RTU or TCP

I have done this addon to integrate my solar system data with our [Home Assistant](https://www.home-assistant.io/) instance.

## Pros:
- data update interval 1 minute (default) or even less instead of default 15 minutes on global.hoymiles.com
- detailed info from separate inverters (ex.: reactive power, power factor) and each panel individually

## Cons:
- tested only on DTU-Pro (see notice below)

Screen from home assistant
<img src="https://github.com/banny310/hoymiles-dtu-homeassistant-addon/raw/master/img/dtu_ha.png" alt="" width="800" />

## How it works

Add-on sets connection with Hoymiles DTU unit and starts listening for incoming data.
When new data is received add-on will transform it and push to mqtt broker.

On start add-on changes server send time configuration on dtu from 15 minutes (dtu default) to 1 minute.
After that data is refreshed every minute in Home Assistant and native Hoymiles dashboard also.

## Dependencies

- Hoymiles DTU (currently tested only on DTU-Pro)
- Home Assistant with Mosquitto Add-on installed (MQTT)

## Installation

1. Copy this repository url https://github.com/banny310/hoymiles-modbus-homeassistant-addon/
2. Add as new repository in Home Assistant add-on store
3. Install add-on
4. Set DTU ip address and port in config tab
```yaml
dtu_mode: 'tcp'
dtu_mode_tcp:
    host: 192.168.88.162
    port: 502
```
5. Start add-on

> ### Notice:
> If you choose TCP mode
> DTU must be connected to your local network by LAN (not by WIFI).\
> Only on LAN interface DTU can be accessed on port 502

## Configuration

An add-on full, default configuration is showed bellow\
You may override that in home assistant yaml addon config (make sure you match structure)
```
dtu_mode = 'rs485'                  # 'rs485' or 'tcp'
dtu_mode_rs485 = {                  # must be provided for mode 'rs485'
    device = /dev/tty.0             # COM port of RS485 reader
    slave_id = 101                  # DTU slave identifer configured by mobile application
}
dtu_mode_tcp = {                    # must be provided for mode 'tcp'
    host = 192.168.1.1
    port = 502
}
mqtt = {
    host = 192.168.1.2
    port = 1883
    username = xxx
    password = xxx-password
}
app = {
    pull_interval = 60                  # time in seconds between each metrics request from DTU
}
```

## Troubleshooting

- DTU only send stats when micros are producing energy. During night inverters are completely off. So if you installed add-on in the eventing, wait until tomorrow.
- In TCP mode DTU must be connected by ethernet (LAN). Only on LAN interface port 502 is open.

## Notice and Warning!

Currently, tested only with DTU-Pro:
- hw: H09.01.02 
- sw: V00.02.15, V00.02.19

If you have a version other than those listed above, run it at your own risk!

#### Licence

> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
