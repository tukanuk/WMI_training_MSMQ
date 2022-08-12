# custom:com.dynatrace.extension.wmi.msmq

## Contents

```
.
├── README.md
├── build_and_upload_sprint.md
├── certs
├── extension
│   ├── alerts
│   ├── dashboards
│   │   └── msmq_overview.json
│   └── extension.yaml
├── poetry.lock
└── pyproject.toml
```

## Instructions

This simple, sample extension monitors MSMQ Services via Dynatrace's Extension 2.0 WMI Datasource. 

The extension itself has no dependencies but development is greatly simplified with the Dynatrace [dt-cli](https://github.com/dynatrace-oss/dt-cli) tool. Instructions to install this tool are available in the [WMI data source tutorial](https://www.dynatrace.com/support/help/extend-dynatrace/extensions20/data-sources/wmi-extensions/wmi-tutorial). 

### Installing MSMQ and an MSMQ Loadgen script

A future version of this repo will include instructions for installing MSMQ on a Windows Server (hint: Add Feature : Messaging Service / MSMQ) and include a loadgen PowerShell script that will simulate MSMQ traffic so you can see something when you deploy this extension.

**TODO: Add this info!** 
### Installing dependencies and a Virtual Environment

If you are familiar with the Python [Poetry](https://python-poetry.org/) tool for package management, there is an included `pyproject.toml` file that contains project dependencies. 

If you follow the [installation instructions](https://python-poetry.org/docs/#installation) for Poetry, you can then install the dependencies by typing:

```zsh
poetry install
```

And then enter the command `poetry shell` to enter an isolated, virtual environment with the `dt-cli` installed.

If none of this interests you, simple type, `pip install dt-cli` and you will globally instal the cli tool.

### Building the extension

Creating and distributing the developer certificate and key is best explained in the [WMI data source tutorial](https://www.dynatrace.com/support/help/extend-dynatrace/extensions20/data-sources/wmi-extensions/wmi-tutorial).

Building and deployment of your extension is explained in [WMI tutorial - extension package](https://www.dynatrace.com/support/help/extend-dynatrace/extensions20/data-sources/wmi-extensions/wmi-tutorial/wmi-tutorial-01) but can be summarized thus:

#### On a linux or MacOS host

1. Set an environment variable called `EXTENSION_TOKEN` with these permissions:
- `Read extension monitoring configurations`
- `Write extension monitoring configurations`
- `Read extension environment configurations`
- `Write extension environment configurations`
- `Read extensions`
- `Write extensions`

2. Set an environment variable called `VERSION` with the current version of the extension
3. Run `dt extension build` with flags in place that define the location of your private key and certificate and where you want your build to be stored (see example).
4. Run `dt extension upload` with flags to set your tenant URL, your api token (via your environment variable) and the path to the `.zip` file containing your extension.

##### Example

```zsh
export EXTENSION_TOKEN=<api_token_with_correct_permissions_here>
export VERSION=1.0.19
dt extension build --target-directory ./build --private-key "/Users/admin/dev/certs/developer.key" --no-dev-passphrase --certificate "/Users/admin/dev/certs/developer.pem"
dt extension upload --tenant-url https://aaa12345.live.dynatracelabs.com --api-token $EXTENSION_TOKEN "./build/custom_com.dynatrace.extension.wmi.msmq-$VERSION.zip"
```



## Latest version: 1.0.19

## Including Functionality
### Topology

This extension will create the following types of entities:
* MSMQ Service (`wmi:msmq_service_instance`)
* MSMQ Service Queue (`wmi:msmq_service_instance_queue`)

### Metrics

This extension will collect the following metrics:
* Split by MSMQ Service Queue, MSMQ Service:
  * Messages in Queue (MSMQ) (`msmq.MessagesinQueue`)
    Count of messages in a MSMQ queue (as Count)
  * Bytes in Queue (MSMQ) (`msmq.BytesInQueue`)
    Total bytes in a MSMQ queue (as Byte)

### Dashboards

This extension is packaged with 1 dashboards which should serve as a starting point for data analysis.
You can find these by opening the Dashboards menu and searching for:

* MSMQ Overview

### Feature sets

Feature sets can be used to opt in and out of metric data collection.
This extension groups together metrics within the following feature sets:

* default:
  * msmq.MessagesinQueue
  * msmq.BytesInQueue

## Variables

Monitoring configurations may expose variables for filtering and adding additional dimensions.
This extension exposes the following variables:

* `queueNameFilter` (variable) - Filter pattern for queue name. Use [WQL](https://docs.microsoft.com/en-us/windows/win32/wmisdk/querying-with-wql) [WHERE](https://docs.microsoft.com/en-us/windows/win32/wmisdk/where-clause) syntax. **EXAMPLES** For all queues use: `Name IS NOT NULL`. For only private queues: `Name LIKE '%private%'`. See https://docs.microsoft.com/en-us/windows/win32/wmisdk/where-clause for full list of options and examples
