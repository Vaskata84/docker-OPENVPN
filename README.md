<p># mkdir DockerVPN<br /># cd DockerVPN<br /># touch Dockerfile<br /># mkdir vpn<br /># touch vpn/VPN-myconfig.ovpn<br />=========================================================<br /># cat Dockerfile</p>
<p>FROM debian:buster</p>
<p># Install required packages<br />RUN apt update &amp;&amp; \<br />apt install openvpn curl -y</p>
<p># Enable TUN/TAP device<br />RUN mkdir -p /dev/net &amp;&amp; \<br />mknod /dev/net/tun c 10 200 &amp;&amp; \<br />chmod 600 /dev/net/tun</p>
<p># Copy OpenVPN configuration file<br />COPY vpn/VPN-myconfig.ovpn /vpn/VPN-BULSAT.ovpn</p>
<p>ENTRYPOINT ["openvpn", "--config", "/vpn/VPN-myconfig.ovpn"]</p>
<p>==========================================================<br />#cat vpn/VPN-myconfig.ovpn<br />client<br />dev tun<br />proto tcp<br />remote XX.XX.XX.XX 1194<br />resolv-retry infinite<br />nobind<br />key-direction 1<br />tls-client<br />persist-key<br />persist-tun<br />ns-cert-type server<br />comp-lzo <br />verb 5</p>
<p></p>
<p></p>
<p># docker build -t my-vpn-image .<br /># docker run --privileged --net=host --name my-vpn-container -d my-vpn-image<br /># docker start my-vpn-container</p>
