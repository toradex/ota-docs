# Accessing the OTA Admin Frontend
---
### navigate to https://app.torizon.io
- Create a user-account  
<img src="./img/createAccount.png"  width="400">   
  
- Log In
<img src="./img/login-screen.png"  width="350">  
    
# Explore :smirk: 
- Provide feedback/shower-thoughts in this slack channel: https://toradex.slack.com/messages/in-devops-ext/

# Provisioning A Device
- In the web frontend, under the devices page, click on the "NEW DEVICE" button.
<img src="./img/createDevice.png"  width="700">   
   
- Copy premade docker command
<img src="./img/copyThis.png" width="700"> 

- Login to a module running torizonCore

- paste and run command  
:pencil: So far this has worked well for me even over serial console. There is the likelihood for data to become garbled/corrupted when copying over serial. Please report this if you experience it.


- For good measure, Restart Aktualizr
```
systemctl restart aktualizr
```
:pencil:  This often works without restarting aktualizr, but sometimes (especially if previously provisioned to an ota backend) aktualizr gets confused.

...or if you want to watch the logs after restarting 
```
systemctl restart aktualizr && journalctl -f -u aktualizr
```
Example logs:
```
root@apalis-imx6-05076305:/var/sota# systemctl restart aktualizr && journalctl -f -u aktualizr
-- Logs begin at Fri 2019-09-13 03:50:29 UTC. --
Sep 13 19:13:19 apalis-imx6-05076305 aktualizr[15032]: Use existing SQL storage: "/var/sota/sql.db"  
Sep 13 19:13:19 apalis-imx6-05076305 aktualizr[15032]: Couldn't import data: "/var/sota/import/root.crt" doesn't      exist.   
Sep 13 19:13:19 apalis-imx6-05076305 aktualizr[15032]: Couldn't import data: "/var/sota/import/client.pem" doesn't exist.   
Sep 13 19:13:19 apalis-imx6-05076305 aktualizr[15032]: Couldn't import data: "/var/sota/import/pkey.pem" doesn't exist.   
Sep 13 19:13:19 apalis-imx6-05076305 aktualizr[15032]: Could not find primary ecu serial, defaulting to empty serial: no more rows available  
Sep 13 19:13:19 apalis-imx6-05076305 aktualizr[15032]: Certificate is not found, can't extract device_id  
Sep 13 19:13:19 apalis-imx6-05076305 systemd[1]: aktualizr.service: Main process exited, code=exited, status=255/EXCEPTION  
Sep 13 19:13:19 apalis-imx6-05076305 systemd[1]: aktualizr.service: Failed with result 'exit-code'.  
Sep 13 19:13:19 apalis-imx6-05076305 systemd[1]: Stopped Aktualizr SOTA Client.  
Sep 13 19:15:27 apalis-imx6-05076305 systemd[1]: Started Aktualizr SOTA Client.  
Sep 13 19:15:27 apalis-imx6-05076305 aktualizr[15078]: Aktualizr version 1.0+gitAUTOINC+505627bbf4 starting  
Sep 13 19:15:27 apalis-imx6-05076305 aktualizr[15078]: Reading config: "/usr/lib/sota/conf.d/20-sota_implicit_prov_ca.toml"  
Sep 13 19:15:27 apalis-imx6-05076305 aktualizr[15078]: Reading config: "/usr/lib/sota/conf.d/40-hardware-id.toml"  
Sep 13 19:15:27 apalis-imx6-05076305 aktualizr[15078]: Reading config: "/etc/sota/conf.d/sota.toml"  
Sep 13 19:15:27 apalis-imx6-05076305 aktualizr[15078]: Bootstrap empty SQL storage  
Sep 13 19:15:27 apalis-imx6-05076305 aktualizr[15078]: Bootstraping DB to version 18  
Sep 13 19:15:27 apalis-imx6-05076305 aktualizr[15078]: Could not find primary ecu serial, defaulting to empty serial: no more rows available  
Sep 13 19:15:29 apalis-imx6-05076305 aktualizr[15078]: ECUs have been successfully registered to the server  
Sep 13 19:15:32 apalis-imx6-05076305 aktualizr[15078]: got SendDeviceDataComplete event  
```
- Your device should now be provisioned and talking with the ota backend!
<img src="./img/is-it-online-arrow.png"  width="350"> 
   

# Release Notes
- Add support for TLS on all OTA endpoints
- Use kong as public ingress instead of Traefik
- Move reverse-proxy behind kong ingress (provides TLS to treehub/oauth2 garage-push)
- Add Support for Deleting Devices
- Add Support for Deleting Fleets
- Updates to UI
  - Landing page
  - Dashboard
  - Feedback page
  - Add provision device button
  - General UI tweaks
- Improvements to auto-provisioning flow. Add info.json to device.zip archive
- Improvements on provisioning container bump version 0.1 -> 0.2
  - Better error handling
  - Dumps the device info on successful provision
- Add fun-names to devices when they are created

# Outstanding for Labs Release
- Stress Testing
- Marketing Survey (link currently doesn't go anywhere)
