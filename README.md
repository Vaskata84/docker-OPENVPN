# mkdir DockerVPN
# cd DockerVPN
# touch  Dockerfile
# mkdir vpn
# touch vpn/VPN-myconfig.ovpn
=========================================================
# cat Dockerfile

FROM debian:buster

# Install required packages
RUN apt update && \
    apt install openvpn curl -y

# Enable TUN/TAP device
RUN mkdir -p /dev/net && \
    mknod /dev/net/tun c 10 200 && \
    chmod 600 /dev/net/tun

# Copy OpenVPN configuration file
COPY vpn/VPN-myconfig.ovpn /vpn/VPN-BULSAT.ovpn

ENTRYPOINT ["openvpn", "--config", "/vpn/VPN-myconfig.ovpn"]

==========================================================
#cat vpn/VPN-myconfig.ovpn
client
dev tun
proto tcp
remote XX.XX.XX.XX 1194
resolv-retry infinite
nobind
key-direction 1
tls-client
persist-key
persist-tun
ns-cert-type server
comp-lzo  
verb 5





# docker build -t my-vpn-image .
# docker run --privileged --net=host --name my-vpn-container -d my-vpn-image
# docker start  my-vpn-container
