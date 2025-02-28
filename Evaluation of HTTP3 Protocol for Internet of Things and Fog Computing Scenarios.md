# Article
[[evaluation_of_http3_protocol_for_iot_and_fog_computing_scenarios.pdf]]

# Summary

# Notes
## Introduction
---
Most iot device use TCP/IP [[evaluation_of_http3_protocol_for_iot_and_fog_computing_scenarios.pdf#page=1&selection=155,14,159,15&color=yellow|p.75]]

MQTT and CoAP two alternatives that is often use [[evaluation_of_http3_protocol_for_iot_and_fog_computing_scenarios.pdf#page=1&selection=163,3,167,39&color=yellow|p.75]]

> ([[evaluation_of_http3_protocol_for_iot_and_fog_computing_scenarios.pdf#page=2&selection=39,0,45,25&color=yellow|p.76]])
> The goal of this paper is to evaluate the use of HTTP for different scenarios in IoT, focusing on the analysis of the data latency between fog nodes and the web server.

Http and MQTT perfomances are altered due to TCP [[evaluation_of_http3_protocol_for_iot_and_fog_computing_scenarios.pdf#page=3&selection=106,18,112,21&color=yellow|p.77]]

HTTP 2 is more performant than HTTP 1 (in most of case) but add complexity due to usage of SSL [[evaluation_of_http3_protocol_for_iot_and_fog_computing_scenarios.pdf#page=3&selection=144,0,148,6&color=yellow|p.77]]

Adapting protocol by using QUIC brings better performances [[evaluation_of_http3_protocol_for_iot_and_fog_computing_scenarios.pdf#page=3&selection=174,33,182,7&color=yellow|p.77]]

QUIC is not yet 100% adapted to IoT devices [[evaluation_of_http3_protocol_for_iot_and_fog_computing_scenarios.pdf#page=4&selection=5,10,11,21&color=yellow|p.78]]

## Methodology
---
Create realistic IoT environment for testing web protocol [[evaluation_of_http3_protocol_for_iot_and_fog_computing_scenarios.pdf#page=4&selection=15,34,19,49&color=yellow|p.78]]

