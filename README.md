# hass-mqtt-mediaplayer (WORK IN PROGRESS)

Allows you to use MQTT topics to fill out the information needed for the Home Assistant Media Player Entity

## Supported Services

[Media Player Entity](https://www.home-assistant.io/integrations/media_player/)

* volume_up
* volume_down
* volume_set
* media_play_pause
* media_play
* media_pause
* media_next_track
* media_previous_track

## Options

| Variables      | Type                                                                      | Default  | Description                                                                       | Expected Payload            | Example                                                                 |
|----------------|---------------------------------------------------------------------------|----------|-----------------------------------------------------------------------------------|-----------------------------|-------------------------------------------------------------------------|
| name           | string                                                                    | required | Name for the entity                                                               | string                      | ```"Musicbee"```                                                        |
| song_title     | string                                                                    | optional | Topic to listen to for the song title                                             | string                      | ```"musicbee/songtitle"```                                              |
| song_artist    | string                                                                    | optional | Topic to listen to for the song artist                                            | string                      | ```"musicbee/artist"```                                                 |
| song_album     | string                                                                    | optional | Topic to listen to for the song album name                                        | string                      | ```"musicbee/album"```                                                  |
| song_volume    | string                                                                    | optional | Topic to listen to for the player volume                                          | string/int (0 to 100)       | ```"musicbee/volume"```                                                 |
| album_art      | string                                                                    | optional | Topic to listen to for the song album art (Must be a base64 encoded string)       | string (base64 encoded url) | ```"musicbee/albumart"```                                               |
| player_status  | string                                                                    | optional | Topic to listen to for the player status (playing/paused)                         | string                      | ```"musicbee/player_status"```                                          |
| vol_topic      | string                                                                    | optional | Topic to publish a change in the player volume                                    | string                      | ```"musicbee/command"```                                                |
| vol_payload    | string                                                                    | optional | Payload to publish (put the keyword "VOL_VAL" where the volume is supposed to be) | string                      | ```"{\"command\":\"volume_set\", \"args\":{\"volume\":\"VOL_VAL\"}}"``` |
| status_keyword | string                                                                    | optional | Keyword used to indicate your MQTT enabled player is currently playing a song     | string                      | ```"true"```                                                            |
| next           | [service call](https://www.home-assistant.io/docs/scripts/service-calls/) | optional | MQTT service to call when the "next" button is pressed                            | N/A                         | * see customize.yaml ex.                                                |
| previous       | [service call](https://www.home-assistant.io/docs/scripts/service-calls/) | optional | MQTT service to call when the "previous" button is pressed                        | N/A                         | * see customize.yaml ex.                                                |
| play           | [service call](https://www.home-assistant.io/docs/scripts/service-calls/) | optional | MQTT service to call when the "play" button is pressed                            | N/A                         | * see customize.yaml ex.                                                |
| pause          | [service call](https://www.home-assistant.io/docs/scripts/service-calls/) | optional | MQTT service to call when the "pause" button is pressed                           | N/A                         | * see customize.yaml ex.                                                |


## Example customize.yaml

```yaml
media_player:  
  - platform: mqtt-mediaplayer
    name: "Musicbee"
    topic:
      song_title: "musicbee/songtitle"
      song_artist: "musicbee/artist"
      song_album: "musicbee/album"
      song_volume: "musicbee/volume"
      album_art: "musicbee/albumart"
      player_status: "musicbee/playing"
    volume:
      vol_topic: "musicbee/command"
      vol_payload: "{\"command\":\"volume_set\", \"args\":{\"volume\":\"VOL_VAL\"}}"
    status_keyword: "true"
    next:
      service: mqtt.publish
      data:
        topic: "musicbee/command"
        payload: "{\"command\": \"next\"}"
    previous:
      service: mqtt.publish
      data:
        topic: "musicbee/command"
        payload: "{\"command\": \"previous\"}"
    play:
      service: mqtt.publish
      data:
        topic: "musicbee/command"
        payload: "{\"command\": \"play\"}"
    pause:
      service: mqtt.publish
      data:
        topic: "musicbee/command"
        payload: "{\"command\": \"pause\"}"

```
