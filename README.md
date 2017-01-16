# Logstash (Nagios) performance data filter

### Description
This filter plugin will index your monitoring performance data into ES, given that you can create for example kibana graphs based on your infrastructure performance data.

The plugin will index data from messagess in the following example:

    [SERVICEPERFDATA]	1484564212	host.domain.lan	/dev/mapper/vg1-root:usage	0.015458	0.000245	DISK OK - free space: /    6182 MB (70% inode=70%);	/=2582MB;7406;8332;0;9258

You have to enable performance data logging in your monitoring engine, @see example for Icinga2.


### Installation logstash
* Move pattern.conf to your logstash pattern directory as nagios-perfdata.conf, usually located at /etc/logstash/patterns/
* Move filter.conf to your logstash filter directory as filter-nagios-perfdata.conf, usually located at /etc/logstash/conf.d
* Reload logstash service

### Enable Perfdata Icinga2
/etc/icinga2/features-enabled/perfdata.conf:

    object PerfdataWriter "syslog" {
        service_format_template =  "[SERVICEPERFDATA]\t$icinga.timet$\t$host.name$\t$service.name$\t$service.execution_time$\t$service.latency$\t$service.output$\t$service.perfdata$"
        host_format_template = "[HOSTPERFDATA]\t$icinga.timet$\t$host.name$\t$host.execution_time$\t$host.output$\t$host.perfdata$"
        rotation_interval   = 86400
    }

Restart Icinga2 service
