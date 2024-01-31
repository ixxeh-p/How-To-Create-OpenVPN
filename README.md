# **openvpn**

_VPN __이란__?_ _공공 인터넷을 통해 가상의 사설 네트워크를 구성하여 프라이빗 통신을 제공하는 기술로 데이터 암호화 __,_ _전용 연결 등 여러 보안 요구 사항을 충족하는 기술이다__._

### CA(인증기관)과, DNS(도메인)서비스가 구성되어 있다고 가정한다.
`
cd /etc/ssl/          //인증서 발급에 Default디렉토리로 이동한다.
`
\n
`
openssl req -out vpn.req -newkey rsa:2048 -nodes -keyout vpn.key -subj '/CN=vpn.test.com'            //vpn.req라는 요청파일을 만들고, VPN.key라는 개인키를 생성하는데 CN(기본이름)을 vpn.test.com을 사용한다.
`
\n
`
openssl ca -in vpn.req -out vpn.crt            //VPN.req(요청파일)로 인증기관 서명을 받은걸 VPN.crt에 저장한다.
`
\\n
`
openssl dhparam 2048 \> dh2048.pem            //디피헬만키를 생성한다 꼭 2048비트여야 한다.
`
\n
`
apt install -y openvpn                        //OpenVPN을 설치한다.
`
\n
`
cp /usr/share/doc/openvpn/example/sample-config-files/server.conf /etc/openvpn          //기본구성파일을 Default 디렉토리로 이동한다.
`
\n
`
vim /etc/openvpn/server.conf          //기본구성파일을 실행한다.
`
\n
`
36 proto udp4                         //36번줄 UDP로 통신할것을 알린다
`
\n
`
78 ca /etc/ssl/ca/cacert.pem          //인증기관의 위치를 알려준다
`
\n
`
79 cert /etc/ssl/vpn.crt              //VPN에서 사용할 인증서를 알려준다
`
\n
`
80 key /etc/ssl/vpn.key               //VPN에서 사용할 인증서 키를 알려준다.
`
\n
`
85 dh /etc/ssl/dh2048.pem             //디피헬만키를 알려준다.
`
\n
`
244 ;tls-auth ta.key 0                //TLS인증을 사용안함으로 바꾼다.
`
\n
`
316 username-as-common-name           //User이름을 CN으로 설정한다.
`
\n
`
317 verify-client-cert none           //클라이언트 개인인증서는 필요없음으로 바꾼다.
`
\n
`
318 plugin openvpn-plugin-auth-pam.so login    //pam login을 지워하게 해주는 Plugin 파일을 import해준다.
`
\n
`
adduser vpnuser
`
\n
`
systemctl restart openvpn@server
`
\n
`
Client(VPN을 연결할 PC)에서 apt install -y Network-manager-openvpn-gnome를 하면 VPN설정창에 OpenVPN이 생길것 이다.
`
