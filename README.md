 Looking Glass (LG) for the Internet Routing Daemon "BIRD" 
 =========================================================

 LG copyright (c) 2013 SUBNETS.RU project (Moscow, Russia)
 Authors: Nikolaev Dmitry <virus@subnets.ru>, Panfilov Alexey <lehis@subnets.ru>

 +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

 This program is free software; you can redistribute it and/or modify
 it under the terms of the GNU General Public License as published by
 the Free Software Foundation; either version 3 of the License

 This program is distributed in the hope that it will be useful,
 but WITHOUT ANY WARRANTY; without even the implied warranty of
 MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 GNU General Public License for more details.

 THIS SOFTWARE IS PROVIDED BY THE AUTHOR AND CONTRIBUTORS ``AS IS'' AND
 ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
 IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
 ARE DISCLAIMED.  IN NO EVENT SHALL THE AUTHOR OR CONTRIBUTORS BE LIABLE
 FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
 DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS
 OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)
 HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
 LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY
 OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF
 SUCH DAMAGE.


================================================================================================================================
    INTRODUCTION
================================================================================================================================

 We migrated from Quagga to BIRD daemon.
 So we needed a LG for BIRD, but we can find only this => https://github.com/sileht/bird-lg/ written on python.
 I (Dmitry) decided to create (for a start) a simple LG on PHP. This is the first version of my LG, so be indulgent :)

+++++++++++++++++++++++++++++++++++++
About Internet Routing Daemon "BIRD" 
+++++++++++++++++++++++++++++++++++++
 The BIRD project aims to develop a fully functional dynamic IP routing daemon.
 
 - Both IPv4 and IPv6
 - Multiple routing tables
 - BGP
 - RIP
 - OSPF
 - Static routes
 - Inter-table protocol
 - Command-line interface
 - Soft reconfiguration
 - Powerful language for route filtering
 
 WWW: http://bird.network.cz/
 
 FreeBSD users can find BIRD daemon in ports tree:
    /usr/ports/net/bird
    /usr/ports/net/bird6

================================================================================================================================
    ABOUT PHP Looking Glass
================================================================================================================================
This is a looking glass for the Internet Routing Daemon "Bird".
Written on PHP: http://php.net/

LG is split in two parts:
 * Web interface;
 * Client for communicate with BIRD socket;

Web interface that request informations from bird nodes.
LG working schemes:

                                                    +++++++++++++     *******************       ****************
                                               +--> + LOCALHOST + --> * bird.client.php * ----> * BIRD sockets *
                                               |    +++++++++++++     *******************       ****************
                                               |
********       ******************************  |    +++++++++++++++     ********************       *******************       ****************
* USER * ----> * http://webserver/index.php *--+--> + REMOTE HOST + --> * TCP-based server * ----> * bird.client.php * ----> * BIRD sockets *
********       ******************************  |    +++++++++++++++     ********************       *******************       ****************
                                               |  
                                               |    +++++++++++++++     ********************       *******************       ****************
                                               +--> + REMOTE HOST + --> * TCP-based server * ----> * bird.client.php * ----> * BIRD sockets *
                                               |    +++++++++++++++     ********************       *******************       ****************
                                               |
                                               |
                                               +--> .....etc......

Files
++++++
 * bird.lg.config.php - configuration file;
 * func.php - php functions;
 * index.php - index file with request form;
 * bird.client.php - php client for connect and run commands on BIRD;
 * js/mt145.js - mootools: http://mootools.net/
 * css/style.css - CSS style file
 * img/indicator.gif - request process image

A few words about bird.client.php
++++++++++++++++++++++++++++++++++
 It can be run separately from web-interface from cli of your server. It gets commands on input and execute them.
 Usage: php bird.client.php -c [ipv4|ipv6]: <command>
 For now bird.client.php suport these commands:
    * bird.client.php -c ipv4: show route for X.X.X.X
    * bird.client.php -c ipv4: show route X.X.X.X/Y
    * bird.client.php -c ipv6: show route for 200X:XXXX::XXXX
    * bird.client.php -c ipv6: show route 200X:XXXX::XXXX/XXX
    * bird.client.php -c ipv4: ping X.X.X.X
    * bird.client.php -c ipv6: ping6 200X:XXXX::XXXX
    * bird.client.php -c ipv4: trace X.X.X.X
    * bird.client.php -c ipv6: trace6 200X:XXXX::XXXX

 So it can run on remote host with the support of TCP-based server. 
 Example of the TCP-based server http://cr.yp.to/ucspi-tcp.html (/usr/ports/sysutils/ucspi-tcp/pkg-descr).
 I don`t attach bird.server.php - this you homework ;)

================================================================================================================================
    INSTALL
================================================================================================================================
Requirements
+++++++++++++
    1. Installed and run BIRD daemon
    2. Installed PHP5 - CLI version and Apache module or CGI version

Installing
+++++++++++
    1. Download last version of LG: http://bird-lg.subnets.ru/
    2. Unpack LG files to your HTTP service folder (such as /usr/local/www)
    3. Edit config file bird.lg.config.php
    4. Edit permissions on BIRD daemon socket
    5. Start browser and open URL such as http://yourserver/lg/index.php

Notes
++++++
    Permissions on BIRD daemon socket: 
	* you must set write permissions on BIRD sockets so user/group who runs HTTP server can write to the BIRD socket
	    exmpl: chmod o=w /path/to/bird.ctl
	* socket permissions will be rewrited after BIRD daemon restarted - keep this in mind

    It should be noted that you can run bird.client.php on localhost with the support of TCP-based server.
    If TCP-based server is started from user root and group wheel than you don`t need to edit permissions on BIRD socket.

================================================================================================================================
    TESTED
================================================================================================================================
Only tested on:
    * OS: FreeBSD, versions: 8.3, 8.4, 9.1
    * BIRD: version 1.3.11
    * PHP: version 5.4.21
    * HTTP: Apache 2.2.25

================================================================================================================================
    SUPPORT
================================================================================================================================
You can request for support or report bug:
    * email: lg@subnets.ru
    * www: 
	    - http://subnets.ru/wrapper.php?p=100
	    - http://subnets.ru/forum/

It necessarily should be attached:
    * your OS version: uname -a
    * your PHP version: php -v
    * a detailed description of your question
Otherwise your request goes to /dev/null.

Please understand that we do not promise to answer your request but we will try to.

================================================================================================================================
    CONCLUSION
================================================================================================================================
Good luck !
����� ! (russian word is in KOI8-R encoding :))

---
With best regards, 
Meganet-2003 IT team
WWW: www.mega-net.ru
