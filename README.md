# INEWS FTP WATCHER

This program is used to crawl inside ftp server read for Inews files. Each file readed is parsed into a json object and produced to kafka broker. The generated json is flaternized to field filter. 

## DOCKER ENVS

Environment with default values:

- MAX_ERROR_COUNT: 10
- SCAN_INTERVAL: 10000ms
- ENCODING: 'utf8'
- FILE_PATTERN: ".*"
- MESSAGE_MAX_SIZE: 1100;
- FIELD_REGEX
    - Regular expression to match the fieldnames on output json.
    - default: '.*'
    - example:
        - single: ".*"
        - multiple: "fields.*|.*org"
- FTP_PATH
    - Used to define the the main folder on FTP server. It will be de base for recursive search unless RECURSIVE=false.
    - default: "/"


- RECUSIVE: 
    - Used to search multiple folders
    - default: true
- KAFKA_CONFIG_FILE_NAME: 'kafka-config.json'

- PROCESSED_FILE_NAME: 
    - Used to store the authentication of processed files
    - defaut: 'processed_files.json'

- FTP_CONFIG_FILE_NAME: 'ftp-config.json'
- CONFIG_PATH: '/config'


## Kafka configuration

Configuration file must exists inside config folder; the configuration file name shoud match KAFKA_CONFIG_FILE_NAME environment variable. This is the basic structure of kafka config: 

```json
{
    "clientId": "my-app",
    "brokers": [
        "broker1: 9094",
        "broker2: 9094",
        "broker3: 9094"
    ],
    "authenticationTimeout": 5000,
    "reauthenticationThreshold": 10000,
    "ssl": true,
    "sasl": {
        "mechanism": "scram-sha-256",
        "username": "user",
        "password": "password"
    },
    "main.input.topic": "default-topic"
}
```

For more information see: https://kafka.js.org/docs/configuration

## INEWS FTP Configuration

The ftp configuration file should be inside config folder. This is the basic of configuration file:

```json
{
    "host": "0.0.0.0",
    "user": "username",
    "password": "pass"
}
```

For more information about configuration ftp client configuration checkout: https://www.npmjs.com/package/promise-ftp