# Proxmox system sensors, to Home Asistant System sensors


I’ve created a simple python script that runs every 60 seconds and sends several system data over MQTT. It uses the MQTT Discovery for Home Assistant so you don’t need to configure anything in Home Assistant if you have discovery enabled for MQTT

It currently logs the following data:

- ProxmoxProd CpuUsage

- ProxmoxProd DiskUse

- ProxmoxProd LastBoot

- ProxmoxProd LastMessage

- ProxmoxProd MemoryUse

- ProxmoxProd SwapUsage

- ProxmoxProd Temperature

- ProxmoxProd UnderVoltage

# System Requirements

You need to have at least **python 3.6** installed to use System Sensors.

# Installation:

1. Clone this repo >> git clone https://github.com/kaciker/Proxmox-to-Mqtt-Hassio-sensors.git
2. cd system_sensors
3. pip3 install -r requirements.txt
4. sudo apt-get install python3-apt
5. Edit settings_example.yaml in "~/system_sensors/src" to reflect your setup and save as settings.yaml:

| Value                           | Required | Default | Description                                                                                                                                     |
| ------------------------------- | -------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| mqtt                            | true     | \       | Details of the MQTT broker                                                                                                                      |
| mqtt:hostname                   | true     | \       | Hostname of the MQTT broker                                                                                                                     |
| mqtt:port                       | false    | 1883    | Port of the MQTT broker                                                                                                                         |
| mqtt:user                       | false    | \       | The userlogin( if defined) for the MQTT broker                                                                                                  |
| mqtt:password                   | false    | \       | the password ( if defined) for the MQTT broker                                                                                                  |
| deviceName                      | true     | \       | device name is sent with topic                                                                                                                  |
| client_id                       | true     | \       | client id to connect to the MQTT broker                                                                                                         |
| timezone                        | true     | \       | Your local timezone (you can find the list of timezones here: [time zones](https://gist.github.com/heyalexej/8bf688fd67d7199be4a1682b3eec7568)) |
| power_integer_state(Deprecated) | false    | false   | Deprecated                                                                                                                                      |
| update_interval                 | false    | 60      | The update interval to send new values to the MQTT broker                                                                                       |
| sensors                         | false    | \       | Enable/disable individual sensors (see example settings.yaml for how-to). Default is true for all sensors.                                      |

6. python3 src/system_sensors.py src/settings.yaml
7. (optional) create service to autostart the script at boot:
   1. sudo cp example_system_sensors.service /etc/systemd/system/system_sensors.service
   2. edit the path to your script path and settings.yaml. Also make sure you replace pi in "User=pi" with the account from which this script will be run. This is typically 'pi' on default raspbian system.
   3. sudo systemctl enable system_sensors.service
   4. sudo systemctl start system_sensors.service

# Home Assistant configuration:

## Configuration:

Install MQTT integration in Home assitant, and all sensors will be availible 
