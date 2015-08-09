# Introduction #

**duat** is a versatile TCP over UDP or TTY proxy-tunnel

# Details #
```


    duat [--option{=<option1>,<option2>,...}] ...
    options:
        --udp-listen-port=<port>                UDP port to listen for incomming connections
    -r  --remote-redir                          let the other side tell where to forward the trafic
        --udp-server=<server:port>              UDP server to tunnel the TCP connection
        --tcp-listen-ports=<port1,port2,...>    TCP ports to listen for connections to forward
                                                (use the last port for transparent proxy)
        --tcp-forwards=<server1:port1,server2:port2,...>
                                                forward TCP <port1>,<port2>,... to these
                                                server:port directions
        --transparent-proxy                     trafic is transparently forwarded
                                                (does not need --tcp-forwards option)
    -a  --iptables-autoconfig                   configure the system to forward all outgoing
                                                internet trafic for the --transparent-proxy option
        --dns-server=<dnsserver>                use <dnsserver> to forward dns request
        --dns-listen-port=<port>                install a DNS-proxy on <port> (normaly <53>)
        --timeout=<sec>                         UDP package will be resendet after <sec>
        --udp-retry=<intents>                   give up resending after this many <intents>
    -s  --size=<bytes>                          UDP packet size (minus header)
    -d  --debug [--debug] ...                   increase log-level
    -l  --logfile=<file>                        log to <file> instead of STDERR
        --remote-log                            sent log to the other tunnel end
        --detach                                detach and run in background
        --client-timeout=<sec>                  disconnect after <sec> (default 600) 0->infinit
    -m  --multimode                             allow more that one client (specify on BOTH ends!)
    -h  --help                                  this help
        --double-ack                            send ACK two times for heavy loss connection
        --tty=<tty-dev1,tty-dev2,...>           use tty additional or alternative to UDP
        --layers=<layer1,layer2,...>            apply these layers to the tunnel
                crypt:blowfish                  Blowfish-encryption
                crypt:RSA                       RSA-encryption
                crypt:DSA                       DSA-encryption
                crypt:<...>                     <...>-encryption (all of perl-Crypt-<...> is supported)
                deflate                         compression (zlib)
                gzip                            compression (zlib)
                uuencode                        uuencoding (no control chars on - helpfull on ttys)
                crc32                           checksum (zlib) (last for better funtionality)
       --recvloss=<0-1>                        for simulation a bad connection (only testing)
        --tty-snow=<some text>                  for testing a bad tty connection (only testing)



Creates proxy tunnel connection trough UDP

expamles
    1.) - very basic setup:
        remote:> duat --udp-listen-port=3333 --tcp-forward=localhost:22
        local :> duat --tcp-listen-ports=2222 --udp-server=<remote-server>:3333
        local :> ssh -p 2222

    2.) - complex settup using a lot of options for transparent forwarding all TCP connections:
        remote:> sudo duat --udp-listen-port=53 --remote-redir --remote-log --dns-server=<ns.local> \
                           --layer=crc32,crypt:blowfish --multimode -d --detach
        local :> sudo duat --tcp-listen-ports=22,1010 --udp-server=<remote-server>:53 --tcp-forward=<ssh.local>:22 \
                           --dns-listen-port=53 --layer=crc32,crypt:blowfish --transparent-proxy --iptables-autoconfig \
                           --multimode -d
        local :> sudo echo "nameserver 127.0.0.1" >/etc/resolv.conf
        local :> firefox http:\\google.com    (or any other TCP client program)
        local :> ssh localhost                (forwarded to <ssh.local>)


```