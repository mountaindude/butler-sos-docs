---
title: "Configuring user sessions"
linkTitle: "User sessions"
weight: 200
description:
---

{{% alert title="Optional" color="primary" %}}
These settings are optional.

If you don't do anything user session metrics are turned on by default.
{{% /alert %}}

## What's this?

A description of what user sessions are is available in the [Concepts section](/docs/concepts/sessions-connections/#sessions).

Detailed user session metrics are retrieved for all virtual proxies specified in the `Butler-SOS.serversToMonitor.servers[].userSessions.VirtualProxies[]` array.

In other words: For each monitored Sense server it is possible to specify which of the server's proxy service's virtual proxies should be monitored with respect to per-user session metrics.  
Right, that's a long sentence...  

Let's try again: For each monitored Sense server, decide which virtual proxies should be monitored.  
Enter those virtual proxies in the `Butler-SOS.serversToMonitor.servers[].userSessions.VirtualProxies[]` array for the server in question. 

{{< notice note >}}
In order to get detailed, per-user and virtual proxy session info you need to

1. Configure the `Butler-SOS.userSessions` section of the config file with general parameters about how often sessions should be polled, user blacklist etc.  
   Don't forget to set `Butler-SOS.userSessions.enableSessionExtract` to true.
2. For each server then set `Butler-SOS.serversToMonitor.servers[].userSessions.enable` to true and specify which virtual proxies should be monitored.

You will only get user session info if you configure both the points above.
{{< /notice >}}

## Settings in main config file

{{< notice tip >}}
The config snippet below comes from the [production_template.yaml](https://github.com/ptarmiganlabs/butler-sos/blob/master/src/config/production_template.yaml) file.

Being a template, it contains examples on how configuration *may* be done - not necessarily how it *should* be done.  
For example, the `LAB/testuser1` and `LAB/testuser2` user are optional and can be changed to something else, or removed all together if not used.  
{{< /notice >}}

```yaml
Butler-SOS:
  ...
  ...
  # Sessions per virtual proxy
  userSessions:
    enableSessionExtract: true      # Query unique user IDs of what users have sessions open (true/false)?
    # Items below are mandatory if enableSessionExtract=true    
    pollingInterval: 30000        # How often (milliseconds) should session data be polled?
    excludeUser:                  # Optional blacklist of users that should be disregarded when it comes to session monitoring.
                                  # Blacklist is only applied to data in InfluxDB. All session data will be sent to MQTT.
      - directory: LAB
        userId: testuser1
      - directory: LAB
        userId: testuser2
  ...
  ...
```
