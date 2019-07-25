Service discovery for [Prometheus](https://prometheus.io/) using devices from [Netbox](https://github.com/digitalocean/netbox).

# Legal

This project is released under MIT license, copyright 2018 ENIX SAS.


# Requirement

This project requires Python 3 and [Pynetbox](https://github.com/digitalocean/pynetbox/).

Also, this project requires 3 custom field to be created into your Netbox instance.
This custom field is named: 
 - `prom_labels` 
 - `prom_target`
 - `prom_target_port`
and can be created in the Netbox admin page (Home -> Extras -> Custom fields) with the following settings:

- Objects: select `dcim > device` and `virtualization > virtual machine`
- Type: `Text`
- Name: `prom_labels`

# Usage

```
usage: netbox-prometheus-sd.py [-h] [-p PORT] [-f CUSTOM_FIELD]
                               url token output

positional arguments:
  url                   URL to Netbox
  token                 Authentication Token
  output                Output file

optional arguments:
  -h, --help            show this help message and exit
  -p PORT, --port PORT  Default target port; Can be overridden using the
                        __port__ label
```

The service discovery script requires the URL to the Netbox instance, an
API token that can be generated into the user profile page of Netbox and a path
to an output file.

 You can also customize the default port on which
the target will point to using the `--port` option. Note that this port can be customized
per target using the `__port__` label set in the custom field.

The output will be generated in the file pointed by the `output` argument.

In the Prometheus configuration, declare a new scrape job using the file_sd_configs
service discovery:

```
- job_name: 'netbox'
  file_sd_configs:
  - files:
    - '/path/to/my/output.json'
```
