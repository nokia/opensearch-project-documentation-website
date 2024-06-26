---
layout: default
title: Network
parent: Supported log types
nav_order: 70
---

# Network

The `network` log type records events that happen in a system's network, such as login attempts and application events.

The following code snippet contains all the `raw_field` and `ecs` mappings for this log type:

```json
 "mappings": [
    {
      "raw_field":"action",
      "ecs":"netflow.firewall_event"
    },
    {
      "raw_field":"certificate.serial",
      "ecs":"zeek.x509.certificate.serial"
    },
    {
      "raw_field":"name",
      "ecs":"zeek.smb_files.name"
    },
    {
      "raw_field":"path",
      "ecs":"zeek.smb_files.path"
    },
    {
      "raw_field":"dst_port",
      "ecs":"destination.port"
    },
    {
      "raw_field":"qtype_name",
      "ecs":"zeek.dns.qtype_name"
    },
    {
      "raw_field":"operation",
      "ecs":"zeek.dce_rpc.operation"
    },
    {
      "raw_field":"endpoint",
      "ecs":"zeek.dce_rpc.endpoint"
    },
    {
      "raw_field":"zeek.dce_rpc.endpoint",
      "ecs":"zeek.dce_rpc.endpoint"
    },
    {
      "raw_field":"answers",
      "ecs":"zeek.dns.answers"
    },
    {
      "raw_field":"query",
      "ecs":"zeek.dns.query"
    },
    {
      "raw_field":"client_header_names",
      "ecs":"zeek.http.client_header_names"
    },
    {
      "raw_field":"resp_mime_types",
      "ecs":"zeek.http.resp_mime_types"
    },
    {
      "raw_field":"cipher",
      "ecs":"zeek.kerberos.cipher"
    },
    {
      "raw_field":"request_type",
      "ecs":"zeek.kerberos.request_type"
    },
    {
      "raw_field":"creationTime",
      "ecs":"timestamp"
    },
    {
      "raw_field":"method",
      "ecs":"http.request.method"
    },
    {
      "raw_field":"id.resp_p",
      "ecs":"id.resp_p"
    },
    {
      "raw_field":"blocked",
      "ecs":"blocked-flag"
    },
    {
      "raw_field":"id.orig_h",
      "ecs":"id.orig_h"
    },
    {
      "raw_field":"Z",
      "ecs":"Z-flag"
    },
    {
      "raw_field":"id.resp_h",
      "ecs":"id.resp_h"
    },
    {
      "raw_field":"uri",
      "ecs":"url.path"
    },
    {
      "raw_field":"c-uri",
      "ecs":"url.path"
    },
    {
      "raw_field":"c-useragent",
      "ecs":"user_agent.name"
    },
    {
      "raw_field":"status_code",
      "ecs":"http.response.status_code"
    },
    {
      "raw_field":"rejected",
      "ecs":"rejected"
    },
    {
      "raw_field":"dst_ip",
      "ecs":"destination.ip"
    },
    {
      "raw_field":"src_ip",
      "ecs":"source.ip"
    },
    {
      "raw_field":"user_agent",
      "ecs":"user_agent.name"
    },
    {
      "raw_field":"request_body_len",
      "ecs":"http.request.body.bytes"
    },
    {
      "raw_field":"service",
      "ecs":"service"
    }
  ]
```