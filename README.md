# "homebridge-http-speaker" Plugin


With this plugin you can create HomeKit speaker services which will redirect commands to the specified http API server.
This could be handy to outsource the "brains" of the speaker to an separate application, maybe in an entirely different 
language.

## Compatibility notice
The speaker servcie was introduced within HAP with the release of iOS 10. It is meant for video doorbells to indicate 
that they support audio output. However Apps like the [Eve App](https://itunes.apple.com/app/elgato-eve/id917695792) 
just display the service and you can control your speaker volume. Though neither the Home App nor Siri support controlling
standalone Speaker services.

## Installation
First of all you should already have installed `Homebridge` on your device. Follow the instructions over at the
[HomeBridge Repo](https://github.com/nfarina/homebridge)

To install the `homebridge-http-speaker` plugin simply run `sudo npm install -g homebridge-http-speaker`

### Configuration

Here is an example configuration. Note that the `mute` section is the only required one
(required by HomeKit Accessory Protocol). `volume` is fully optional. `power` was my decision to include it in the code.
The power attribute is not foreseen for the speaker but the Eve App manages to handle this 'abnormal' characteristic.
We will see what the Home App will do with it.


Every call needs to be status code `200` if successful. The `statusUrl` call of `mute` (and of `power`) expects an `0`
or `1` for `off` or `on` with no html markup inside the body of the response. The `statusUrl`of `volume` expects a
value from 0 to 100 with no html markup inside the body. The `%s` in the `setUrl` call will be replaced with the volume.

```
    "accessories": [
        {
            "accessory": "HTTP-SPEAKER",
            "name": "Your Speaker name",
            
            "mute": {
                "onUrl": "http://localhost/api/muteOn",
                "offUrl": "http://localhost/api/muteOff",

                "statusUrl": "http://localhost/api/muteStatus"
            },
            
            "volume": {
                "statusUrl": "http://localhost/api/volumeStatus",
                "setUrl": "http://localhost/api/volumeUpdate/%s"
            },
            
            "power": {
                "statusUrl": "http://localhost/api/powerStatus",
                "onUrl": "http://localhost/api/powerOn",
                "offUrl": "http://localhost/api/powerOff"
            }
        }
    ]
```
