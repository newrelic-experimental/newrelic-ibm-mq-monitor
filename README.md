⚠️ We recommend using  the official New Relic  [integration for IBM MQ](https://docs.newrelic.com/docs/infrastructure/host-integrations/host-integrations-list/ibmmq-monitoring-integration/) to collect both SNMP and Network Flows. This integration is no longer being maintained. 

[![New Relic Experimental header](https://github.com/newrelic/opensource-website/raw/master/src/images/categories/Experimental.png)](https://opensource.newrelic.com/oss-category/#new-relic-experimental)

![GitHub forks](https://img.shields.io/github/forks/newrelic-experimental/newrelic-ibm-mq-monitor?style=social)
![GitHub stars](https://img.shields.io/github/stars/newrelic-experimental/newrelic-ibm-mq-monitor?style=social)
![GitHub watchers](https://img.shields.io/github/watchers/newrelic-experimental/newrelic-ibm-mq-monitor?style=social)

![GitHub all releases](https://img.shields.io/github/downloads/newrelic-experimental/newrelic-ibm-mq-monitor/total)
![GitHub release (latest by date)](https://img.shields.io/github/v/release/newrelic-experimental/newrelic-ibm-mq-monitor)
![GitHub last commit](https://img.shields.io/github/last-commit/newrelic-experimental/newrelic-ibm-mq-monitor)
![GitHub Release Date](https://img.shields.io/github/release-date/newrelic-experimental/newrelic-ibm-mq-monitor)


![GitHub issues](https://img.shields.io/github/issues/newrelic-experimental/newrelic-ibm-mq-monitor)
![GitHub issues closed](https://img.shields.io/github/issues-closed/newrelic-experimental/newrelic-ibm-mq-monitor)
![GitHub pull requests](https://img.shields.io/github/issues-pr/newrelic-experimental/newrelic-ibm-mq-monitor)
![GitHub pull requests closed](https://img.shields.io/github/issues-pr-closed/newrelic-experimental/newrelic-ibm-mq-monitor)


# [Warning: This is a Deprecated Repository] New Relic integration for IBM MQ (newrelic-ibm-mq-monitor)

The newrelic ibm mq monitor allows you to monitor the performance of MQ Objects like channels and queues while also enabling easy alerting.

## Installation and Starting the monitor

1. Download the zip file from [the latest release](https://github.com/newrelic-experimental/newrelic-ibm-mq-monitor/releases/latest) and unzip to a suitable location on your server.
2. Update the `APP_HOME` and `MQ_LIB` variables in the appropriate startup script for your operating system (`start.bat` for Windows or `start.sh` for Linux) to point to the appropriate locations:
   - `APP_HOME`: The directory where you unzipped the monitor.
   - `MQ_LIB`: The directory where your MQ client libraries are stored. **(Note: These are not included in the monitor distribution.)**
4. Configure `plugin.json` according to the **Configuration** section below.
5. Run the appropriate startup script for your operating system (`start.bat` for Windows or `start.sh` for Linux).

## Configuration

Rename the plugin.template.json to plugin.json. Edit all parameters to your environment. 

### Global Properties

The "global" object in plugin.json contains general properties application to the monitor.

- **account_id**: your new relic account id. You can find it in the URL that you use to access newrelic. For example: https://rpm.newrelic.com/accounts/{accountID}/applications
- **insights_mode**:
	- The insights_insert_key provided here will be used to post metrics to New Relic.
- **proxy**: Enter the proxy setting in this section if a proxy is required. 
	- "proxy_host": the proxy host name or ip addresss
	- "proxy_port": the proxy port
	- "proxy_username": (optional) proxy user name
	- "proxy_password": (optional) proxy password
- **queueIgnores**:  An array of "ignoreRegEx" objects. The value of the object is a regular expression. Any queue name on any queue manager that matches the regular expression will be ignored (i.e. no metrics collected). The array can contain any number of entries.
- **queueIncludes**: Overrides queueIgnores with same format. This allows wildcard excludes but then the ability to explicitly include specific queues here.

### Instance Properties

The **agents** array in **plugin.json** contains objects each of which contain properties specific to an IBM MQ Queue Manager that is to be monitored.
Each instance can have the following properties

- **name**: any descriptive name for the queue manager
- **host**: hostname or IP for the queue manager
- **port**: port number that the queue manager is listening on
- **queueManager**: the name of the MQ queue manager to connect to.
- **channel**: channel name used to connect to the queue manager. Typically you can use SYSTEM.DEF.SVRCONN
- **username**: username used to connection 
- **password"**: password used to connection 

### Password Obfuscation
For additional security, this integration supports the use of an obfuscated proxy password with the proxy_password_obfuscated attribute. Similarly, it also supports obfuscating any other password attribute by appending "_obfuscated" to the attribute name and providing an obfuscated value. 

The obfuscated proxy password is generated using the following [New Relic CLI](https://github.com/newrelic/newrelic-cli)  command:

```

newrelic agent config obfuscate --key OBSCURING_KEY --value "CLEAR_TEXT_PROXY_PASSWORD"

```

The obscuring key must also be configured by setting the NEW_RELIC_CONFIG_OBSCURING_KEY environment variable.

## Logging

The logging configuration can be controlled using the logback configuration file- ./config/logback.xml

Edit the following block of XML at the end of the logback.xml to change the log level (possible values are INFO, DEBUG, ERROR) and the log ouput(possible values are STDOUT, FILE)
```
    <logger name="com.newrelic" level="DEBUG" additivity="false">
        <appender-ref ref="STDOUT" />
    </logger>
```

## Metrics and Dashboarding
All metrics collected by this plugin are reported as events of type "IBMMQSample". 


## Building

1. Build the project: `gradle clean build`
2. Copy the resulting contents of `build/distributions` folder into newrelic-ibm-mq-monitor folder.

## Support

New Relic has open-sourced this project. This project is provided AS-IS WITHOUT WARRANTY OR DEDICATED SUPPORT. Issues and contributions should be reported to the project here on GitHub.

We encourage you to bring your experiences and questions to the [Explorers Hub](https://discuss.newrelic.com) where our community members collaborate on solutions and new ideas.


## Contributing

We encourage your contributions to improve Salesforce Commerce Cloud for New Relic Browser! Keep in mind when you submit your pull request, you'll need to sign the CLA via the click-through using CLA-Assistant. You only have to sign the CLA one time per project. If you have any questions, or to execute our corporate CLA, required if your contribution is on behalf of a company, please drop us an email at opensource@newrelic.com.

**A note about vulnerabilities**

As noted in our [security policy](../../security/policy), New Relic is committed to the privacy and security of our customers and their data. We believe that providing coordinated disclosure by security researchers and engaging with the security community are important means to achieve our security goals.

If you believe you have found a security vulnerability in this project or any of New Relic's products or websites, we welcome and greatly appreciate you reporting it to New Relic through [HackerOne](https://hackerone.com/newrelic).

## License

[New Relic IBM MQ monitor] is licensed under the [Apache 2.0](http://apache.org/licenses/LICENSE-2.0.txt) License.

[New Relic IBM MQ monitor] also uses source code from third-party libraries. You can find full details on which libraries are used and the terms under which they are licensed in the third-party notices document.]
