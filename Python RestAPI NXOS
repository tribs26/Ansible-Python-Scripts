#!usr/bin/env python

import requests
from device_info import nexus
import json
import urllib3
from pprint import pprint

# Silence the insecure warning due to SSL Certificate
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)


headers = {
              'content-type': "application/json",
              'x-auth-token': ""
          }

def nexus_login(host, port, username, password):
    """
    Use the REST API to Log into nexus and retrieve token
    """
    url = "https://{}:{}/api/aaaLogin.json".format(host, port)
    payload = "{\r\n    \"aaaUser\": {\r\n        \"attributes\": {\r\n            \"name\": \"admin\",\r\n            \"pwd                                                                                                                \": \"cisco@123\"\r\n        }\r\n    }\r\n}"

    # Make Login request and return the response body
    response = requests.request("POST", url, data = payload, headers=headers, verify=False)
    return response.json()


def network_device_list(host, cookie):

#    Use the REST API to retrieve the status of eth1/2

    payload = {}
    url = "https://{}/api/node/mo/sys/intf/phys-[eth1/2].json".format(host)
    headers["Cookie"] = cookie

    # Make API request and return the response body
    response = requests.request("GET", url, headers=headers, data = payload, verify=False)
    return response.json()

# Entry point for program
if __name__ == '__main__':
    # Log into the device to get the token
    token = nexus_login(nexus["host"], nexus["port"], nexus["username"], nexus["password"])
    token1 = token['imdata'][0]['aaaLogin']['attributes']['token']
    sessionId = token['imdata'][0]['aaaLogin']['attributes']['sessionId']
    cookie = "nxapi_auth=" + sessionId + ";" + " " + "APIC-cookie=" + token1
# Get the list of devices
    devices = network_device_list(nexus["host"], cookie)
    MTU = devices['imdata'][0]['l1PhysIf']['attributes']['mtu']
    pprint("MTU of eth1/2 interface is: {}".format(MTU))
