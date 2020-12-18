# Vulnerability reports for Cisco IOS, NX-OS and ACI 

This Ansible playbook collects the software version of all devices in the inventory, queries the [Cisco PSIRT OpenVuln API](https://developer.cisco.com/psirt/) and renders a markdown report.

## API Keys

You need to generate a client_key/id and client_secret via the [Cisco API Console](https://apiconsole.cisco.com/) using your CCO Login under `My Apps & Keys`, `Register a new App`.
Select OAuth2.0 Client Credentials and the Cisco PSIRT checkbox to receive the necessary API keys.

## Input Data

The playbook needs certain input data to do it's job, not all of them are mandatory.

### 1. API Credentials (Mandatory)

Under the vars section of the second play, or via external extra vars:

    client_key: <client_key provided by the API Console> 
    client_secret: <client_secret provided by the API Console>

### 2. A list of hosts named host_list

Under the vars section of the second play, or via external extra vars.  
Useful for air gapped environments or simple API testing.

    host_list:
          - { name: aci, version: 14.2(5k), tag: Test_ACI }
          - { name: nxos, version: 7.0(3)I7(7), tag: Test_NXOS }
          - ...

### 3. A list of Cisco ACI credentials named aci 

Under the vars section of the second play, or via external extra vars.  
In case you'd like to automagically include the software version of all leaf/spine switches of one or more ACI fabrics.   

    aci:
      - { apic: <APIC-Fabric1-URL/IP> , aci_user: <username1>, aci_pass: <password1> }
      - { apic: <APIC-Fabric2-URL/IP> , aci_user: <username2>, aci_pass: <password2> }

### 4. A list of known security advisories via an external file known_vuln.yml

This is just a flat dictionary with the advisoryId as key and an optional comment as value, to document the status and / or countermeasures for each vulnarbility in your organisation. This is useful to keep track of security improving activities and to quickly spot when new / unseen advisories arive. The format of the file looks like:

    ---
    cisco-sa-20200226-fxos-nxos-cdp: CDP not in use / disabled
    cisco-another-advisoryId: We're in the process of updating to version ...
    ...

## Run 



Useful to 