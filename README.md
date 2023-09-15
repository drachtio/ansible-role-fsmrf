# ansible-role-fsmrf [![Build Status](https://secure.travis-ci.org/davehorton/ansible-role-fsmrf.png)](http://travis-ci.org/davehorton/ansible-role-fsmrf)

This is an ansible role for building a [Freeswitch](https://freeswitch.org/) v1.10.10 that is configured to work with the [drachtio-fsmrf](https://github.com/davehorton/drachtio-fsmrf) module.  Additionally, all of the modules in [drachtio-freeswitch-modules](https://github.com/drachtio/drachtio-freeswitch-modules) are installed as part of this role.


## Role variables

Available variables are listed below, along with default values (see defaults/main.yml):

```
build_with_extra: false
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
    - name: "build_with_extra"
      prompt: "Include the grpc modules (mod_google_transcribe, mod_google_tts, mod_dialogflow)?"
      private: no
      default: false
  roles:
    - ansible-role-fsmrf
```
