# v-rising-wine-docker-image

This docker image is supposed to run a v rising server.
At the time of writing the server is only available as a windows executable.
This project therefore uses wine to run the server.

The image is available on [docker hub (mephi00/v-rising-wine)](https://hub.docker.com/r/mephi00/v-rising-wine).

# Setting up the container

In order to access the Server, you need to expose the specified server port on the udp protocol. In docker this is achieved by the argument _-p \<external port>:\<internal port>/udp_. For the server to be visible on the public listing, the specified query port has to be exposed by the udp protocol as well. This is achieved the same way as described above. <br>
Addionally, the afore mentioned ports have to be publicly exposed in your router settings.<br>
For an example docker compose file please take a look [here](./docker-compose.yaml)
<br>

Please note, that upon first start up, the server initialises the world, which takes about 10 minutes, maybe longer. Additionally, the container begins by updating the game server, so you are not relying on a new image in order to update the server to a new version. This also takes some time, so don't panic when your server isn't immediately available.

# Configurations

The server can be configured using environment variables or by providing configuration files

## Using environment variables

### Variables to configure Settings in ServerHostSettings.json

| Variable Name           | Datatype | Description                                   | Default Value    |
| ----------------------- | :------: | --------------------------------------------- | ---------------- |
| V_RISING_NAME           |  string  | Name of the server                            | _Required Value_ |
| V_RISING_SAVE_NAME      |  string  | Name of the world save                        | _Required Value_ |
| V_RISING_PASSW          |  string  | The server password                           | _Required Value_ |
| V_RISING_PUBLIC_LIST    | boolean  | Whether to list the server publicly           | _Required Value_ |
| V_RISING_DESC           |  string  | Description of the server                     | Empty string     |
| V_RISING_PORT           |   int    | Port of the server                            | 9876             |
| V_RISING_QUERY_PORT     |   int    | Port für das Query Interface                  | 9877             |
| V_RISING_MAX_USER       |   int    | Num of max concurrent players                 | 40               |
| V_RISING_MAX_ADMIN      |   int    | Num of max concurrent player using admin menu | 4                |
| V_RISING_SETTING_PRESET |  string  | Setting preset provided by Stunlock Studios   | Empty String     |

### Variables to configure settings in ServerGameSettings.json

| Variable Name                        |         Datatype          | Description                                       | Default Value |
| ------------------------------------ | :-----------------------: | ------------------------------------------------- | :-------------: |
| V_RISING_GAME_MODE                   |      PvP or PvE       | Whether the game is pvp or pve                    | PvP         |
| V_RISING_MAX_HEALTH_MOD              |           float           | Modifier for the health of player characters      | 1.0           |
| V_RISING_MAX_HEALTH_GLOBAL_MOD       |           float           | Modifier for the health of all entities           | 1.0           |
| V_RISING_RESOURCE_YIELD_MOD          |           float           | Modifier for the resource yield                   | 1.0           |
| V_RISING_DAY_DURATION_SECONDS        |           float           | Duration of a day in seconds                      | 1080.0        |
| V_RISING_DAY_START_HOUR              |            int            | Hour of the (in-game) 24h day for the sun to rise | 9             |
| V_RISING_DAY_END_HOUR                |            int            | Hour of the (in-game) 24h day for the sun to set  | 17            |
| V_RISING_TOMB_LIMIT                  |            int            | Limit of number of Tombs in a castle              | 12            |
| V_RISING_NEST_LIMIT                  |            int            | Limit of number of Vermite Nests in a castle      | 4             |
| V_RISING_DEATH_CONTAINER_PERMISSIONS | Anyone or ClanMembers | Who can pick up items of a dead vampire           | Anyone      |
| V_RISING_CLAN_SIZE                   |            int            | Maximum number of players in a clan               | 4             |

## Using configuration files

For a fully custom experience, providing your own setting files is recommended. <br>
The files _ServerGameSettings.json_ and _ServerHostSettings.json_ must be present in the folder _/var/settings/_. <br>
It is possible to only provide one of these setting files. If this is the case, please note that a customisation using environment variables is still possible for the file, not provided. _Please be aware, that the required variables are still required, when you do not provide the ServerHostSettings.json file._ <br><br>

### Example files

Example files can be found [here](./examples), these contain the default settings by Stunlock Studios.

# Save File Location

The Save folder is /saves. In order to have access to the save files for an easy backup, mount a folder to /saves or it's subfolders, according to V Risings folder structure.<br>
The mount option can also be used to transfer your save files from a different installation or playthrough. For this to work be aware of the folder structure and, that the save name has to be set to your added save state.
