# nginx

To access a guest (VM) nginx from a host, set a port forwarding in Virtualbox with name `nginx`, host port `80`, guest port `80`.

## Install
`add-apt-repository ppa:nginx/stable` - Add 3rd party repo.  
`apt-get update` - Refresh packages list.  
`apt-get install nginx` - Install the web server.  
