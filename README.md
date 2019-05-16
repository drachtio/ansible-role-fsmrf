# ansible-role-fsmrf [![Build Status](https://secure.travis-ci.org/davehorton/ansible-role-fsmrf.png)](http://travis-ci.org/davehorton/ansible-role-fsmrf)

This is an ansible role for building a [Freeswitch](https://freeswitch.org/) v1.8 that is configured to work with the [drachtio-fsmrf](https://github.com/davehorton/drachtio-fsmrf) module.  The [mod_audio_fork](https://github.com/davehorton/drachtio-freeswitch-modules/blob/master/modules/mod_audio_fork/README.md) module is built and installed as part of this role.

Additionally, by setting the `build_with_grpc` variable to true, these additional modules will be built and included:
- [mod_google_transcribe](https://github.com/davehorton/drachtio-freeswitch-modules/tree/master/modules/mod_google_transcribe)
- [mod_google_tts](https://github.com/davehorton/drachtio-freeswitch-modules/tree/master/modules/mod_google_tts)
- [mod_dialogflow](https://github.com/davehorton/drachtio-freeswitch-modules/tree/master/modules/mod_dialogflow)

## Freeswitch modules
Because this is designed for use as a media server via [drachtio-fsmrf](https://github.com/davehorton/drachtio-fsmrf), a very limited list of modules are built and installed:
```xml
  <load module="mod_console"/>
  <load module="mod_audio_fork"/>
  <load module="mod_logfile"/>
  <load module="mod_cdr_csv"/>
  <load module="mod_event_socket"/>
  <load module="mod_sofia"/>
  <load module="mod_rtc"/>
  <load module="mod_commands"/>
  <load module="mod_conference"/>
  <load module="mod_dptools"/>
  <load module="mod_dialplan_xml"/>
  <load module="mod_httapi"/>
  <load module="mod_spandsp"/>
  <load module="mod_g723_1"/>
  <load module="mod_g729"/>
  <load module="mod_amr"/>
  <load module="mod_opus"/>
  <load module="mod_sndfile"/>
  <load module="mod_native_file"/>
  <load module="mod_local_stream"/>
  <load module="mod_tone_stream"/>
  <load module="mod_say_en"/>
```

## Role variables

Available variables are listed below, along with default values (see defaults/main.yml):

```
build_with_grpc: false
```
set to true if you want to build with the google grpc library and then build related google modules as described above

```
grpc_version: v1.20.0
```
The version of grpc to build

```
freeswitch_sources_path: /usr/local/src/freeswitch/
```
path to the FreeSwitch source directory

```
freeswitch_path: /usr/local/freeswitch/
```
path to the FreeSwitch install directory

```
freeswitch_secret: ClueCon 
```
freeswitch secret

```
freeswitch_sip_port: 5080
freeswitch_dtls_sip_port: 5081
```
sip ports to listen on

```
freeswitch_socket_acl:
  - 127.0.0.1/32
```
add any addresses (in cidr format) that should be allowed to connect to event socket

```
freeswitch_modules_template: ../templates/modules.conf 
```
modules.conf file used for FreeSwitch compilation

```
freeswitch_configure_command: configure --with-lws=yes
```
freeswitch configure command - you can add option

```
freeswitch_log_conf_template: ../templates/logfile.conf.xml

```
freeswitch log configuration file template

## Example playbook
```
---
- hosts: all
  become: yes
  vars_prompt:
    - name: "build_with_grpc"
      prompt: "Include the grpc modules (mod_google_transcribe, mod_google_tts, mod_dialogflow)?"
      private: no
      default: false
  roles:
    - ansible-role-fsmrf
```
