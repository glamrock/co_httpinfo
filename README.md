co_httpinfo
===========

_Olsr's httpinfo plugin customized for Commotion's dashboard_

##About
This plugin for olsrd creates a page on the fly to display common network stats.

Information returned includes:
* olsrd routes
* local configuration
* uptime
* detailed link status
* neighbors' status
* IP and MAC addresses

##Use & options
Default port if not defined is 1978. You can set a custom port number to avoid conflicts as necessary. Don't forget to allow this port in your firewall if you'd like to access it from a remote host.

By setting IP address, you are defining which addresses are allowed to access the page. To allow access from all IP addresses, use ` PlParam   "Net" "0.0.0.0 0.0.0.0"` For local networks, this should not be a problem. For networks that are connected to the internet, more security is needed.  (So perhaps consider *only* allowing your local IP to access the dashboard).

The parameter `"Net" "[IP net] [IP mask]"` is used to allow access from entire net-ranges.

### Example config 1

    LoadPlugin "olsrd_httpinfo.so.0.1"
    {
        PlParam     "port"   "80"
        PlParam     "Host"   "163.24.87.3"
        PlParam     "Host"   "80.23.53.22"
        PlParam     "Net"    "10.0.0.0 255.0.0.0"
        PlParam     "Net"    "192.168.0.0 255.255.0.0"
    }

This will run the webserver on port 80(the normal
HTTP port) and allow access from the hosts 163.24.87.3
and 80.23.53.22 and the networks 10.0.0.0/8 and
192.168.0.0/16.
access is always allowed from 127.0.0.1(localhost).

### Example config 2

    LoadPlugin "olsrd_httpinfo.so.0.1"
    {
        PlParam     "port"   "80"
        PlParam     "Net"    "192.168.*.* 255.255.0.0"
    }

This will run the service on port 80 and allow all access from the local 192.168.*.* network. Notably, this will work even if there's another webserver installed, as it doesn't rely on the 127.0.0.1 address (which is whitelisted by default).

## Admin Interface
This is a web page that will let the user change olsrd settings in realtime.

To compile with this *experimental* feature pass `ADMIN_INTERFACE=1` as an argument to make (eg. `make ADMIN_INTERFACE=1`)

##Compiling
httpinfo is compatible with Windows, Debian-based linux distros, and FreeBSD. Please use the command line.

*Windows*
    make OS=win32
    make install

*Linux*
    make
    make install

*FreeBSD*
    gmake OS=fbsd
    gmake install

## Problem?
If, after compiling, httpinfo won't work, there are several possibilities: 

* Not including the module in the config (surprisingly common!)
* Not using the port you've set in the config file
* Setting an IP address that isn't your local IP (remember: you can include many IP addresses in the config)
* olsrd version mismatch (Option A: find it in the httpinfo code and change it to match your olsrd version. Option B: update olsrd and this plugin)

## Credits
Andreas Tonnesen(andreto@olsr.org) created the original httpinfo plugin, and there were some hacks by Griffin Boyce (griffin@opentechinstitute.org)
