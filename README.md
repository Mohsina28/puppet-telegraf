## Overview

[![Build Status](https://travis-ci.org/rplessl/puppet-telegraf.png)](https://travis-ci.org/rplessl/puppet-telegraf)

This Puppet module installs and manages [InfluxDB Telegraf](https://github.com/influxdb/telegraf). 

Use this puppet module to install and configure [InfluxDB Telegraf](https://github.com/influxdb/telegraf) with version 0.2.0 and newer.

## Module Description

The module installs the telegraf package from the provided repositories and installs the basic configuration file and reconfigures this setup based on your whishes.

Supported Linux distributions are Debian based (Ubuntu, Debian) and RedHat based (CentOS, RHEL).

The installed package for telegraf will be fetched from
  a) the provided package from get.influxdb.com or
  b) the provided package from your own repository (apt repository, aptly, yum)

## Setup

### Setup Requirements

puppet-telegraf requires only the [wget](https://forge.puppetlabs.com/maestrodev/wget) module when the parameter `install_from_repository` is set to false. Furthermore the `stdlib` module is required.

### Beginning with telegraf

Include the class and set the InfluxDB parameters.

```
class { 'telegraf':
    install_from_repository   => false,
    outputs_influxdb_enabled  => true,
    outputs_influxdb_urls     => ['http://localhost:8086'],
    outputs_influxdb_database => 'telegraf',
    outputs_influxdb_username => 'telegraf',
    outputs_influxdb_password => 'metricsmetricsmetricsmetrics',
}
```

This telegraf module supports the following configuration options:

```
class { 'telegraf':
    ensure                    => 'installed',
    version                   => '0.2.0',
    install_from_repository   => true,
    config_base_file          => '/etc/opt/telegraf/telegraf.conf',
    config_directory          => '/etc/opt/telegraf/telegraf.d',

    # [outputs.influxdb] section of telegraf.conf
    outputs_influxdb_enabled  => true,
    outputs_influxdb_urls     => ['http://localhost:8086'],
    outputs_influxdb_database => 'telegraf',
    outputs_influxdb_username => 'telegraf',
    outputs_influxdb_password => 'metricsmetricsmetricsmetrics',

    # [tags] section of telegraf.conf
    tags                      => {
      virtual            => $::virtual,
      lsbdistdescription => $::lsbdistdescription,
      environment        => $::my_own_facter_environment,
      location           => $::my_own_facter_location,
    }

    # [agent]
    agent_hostname            => $::hostname,
}
```

## Development 

1. Fork it (https://github.com/rplessl/puppet-telegraf/fork)
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request

Wish: Make sure your Pull Requests passes the Rspec tests.

## Testing Code Enhancement (rspec)

Testing is done with [rspec](http://rspec-puppet.com/).

## Testing Setup with Vagrant

Install and setup vagrant [https://docs.vagrantup.com/v2/installation/index.html](as described here).

Fetch virtual machines:
```ShellSession
vagrant box add puppetlabs/ubuntu-14.04-64-puppet --insecure
vagrant box add puppetlabs/centos-6.6-64-puppet   --insecure
vagrant box add puppetlabs/centos-7.0-64-puppet   --insecure
vagrant box add puppetlabs/debian-7.8-64-puppet   --insecure
vagrant box add markusperl/debian-8.0-jessie-64-shrinked-puppet --insecure
```

Add vagrant puppet support and run tests:
```ShellSession
bundle install
bundle exec librarian-puppet install
vagrant up
```

## License

Licensed under the MIT License.
Copyright 2015 Roman Plessl (@rplessl)

See [LICENSE](https://github.com/rplessl/puppet-telegraf/blob/master/LICENSE) File





