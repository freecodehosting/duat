# Introduction #

this is a stub for a proper documentation. Please have a look into the perl-file for more options


# Details #
quick start example

server:    duat --udp-listen-port=4554 --remote-redir --debug --dns-server=localhost

client:     duat --tcp-listen-ports=220,4000 --tcp-forwards=myssh.server.net:22 --transparent-proxy --iptables-autoconfig --dns-listen-port=53