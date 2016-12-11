### lab cuối kỳ (Không chính thức)

# Để xóa cấu hình trên vyatta thì thay  set thành delete trước các lệnh đã cấu hình set
# Sử dụng vi cơ bản

- Mở file với vi:
	
	vi [tên file]

- Đóng và lưu file: nhấn esc và :x hoặc :wq

- Đóng và không lưu:  nhấn esc và :q hoặc :q!

- Đi đến dòng bất kỳ: nhấn esc và  :số_dòng

- Copy paste: Dùng chuột cho lẹ :D

# Những từ nằm trong dấu [] là người dùng muốn thay đổi tùy ý

# Tạo user và đổi password để test ví dụ như FTP hay mail server

- #useradd [tên username] 			; Tạo user
- #passwd [tên username muốn đổi pass]  	; Đổi mật khẩu

# Cài phần mềm trên CentOS nhớ chọn đúng và đầy đủ tên phần mềm. Ví dụ ghi vsftpd dùng cho FTP thì file đầy đủ là vsftpd-2.2.2-11.el6_4.1.x86_64.rpm





### Bắt đầu cấu hình============================================

# cấu hình IP (Chọn đúng card mạng - xem địa chỉ MAC để pit card nào là của eth nào)

vyatta 1

vyatta$conf
vyatta#set interface ethernet eth0 address 192.168.12.1/24
vyatta#set interface ethernet eth1 address  10.0.0.1/8

vyatta 2

vyatta$conf
vyatta#set interface ethernet eth0 address 192.168.12.2/24
vyatta#set interface ethernet eth1 address  20.0.0.1/8


# cấu hình định tuyến============================================

vyatta 1

vyatta#set protcol ospf ares 0.0.0.0 network 10.0.0.0/8
vyatta#set protcol ospf ares 0.0.0.0 network  192.168.12.0/24

vyatta 2

vyatta#set protcol ospf ares 0.0.0.0 network 20.0.0.0/8
vyatta#set protcol ospf ares 0.0.0.0 network  192.168.12.0/24


# cấu hình dhcp=============================================

vyatta 1

vyatta#set service dhcp-server
vyatta#set service dhcp-server share-network-name abc subnet 10.0.0.0/8
vyatta#set service dhcp-server share-network-name abc subnet 10.0.0.0/8 start 10.0.0.11 stop 10.0.0.100
vyatta#set service dhcp-server share-network-name abc subnet 10.0.0.0/8 dns-server 20.0.0.10
vyatta#set service dhcp-server share-network	-name abc subnet 10.0.0.0/8 default-router 10.0.0.1
vyatta#set service dhcp-server share-network-name abc subnet 10.0.0.0/8 lease 60000


# Lưu ý:  Áp dụng cấu hình và lưu cài đặt (cả 2 vyatta :P)=======================

vyatta#commit
vyatta#save


## cấu hình các dịch vụ===============================================

- Kiểm tra phần mềm hỗ trợ dịch vụ đã được cài đặt chưa với lệnh

	rpm -qa | grep [tên phần mềm]

- Lệnh cài đặt phần mềm (nhớ thêm file iso và cài với quyền root) nếu chưa đc cài trên máy

	rpm -ivh /media/CentOS_6.5_Final/Packages/[tên phần mềm]

# SSH 		(cần cài phần mềm *openssh*)==============================

- Cấu hình file /etc/ssh/sshd_config

	+ Bỏ dấu *#* dòng 13

- Khởi động lại dịch vụ

	+ service sshd restart 

- Khởi động cùng hệ thống

	+ chkconfig ssh on


# HTTP 	(cần cài phần mềm *httpd*)=================================

- Tạo thư mục chứa trang web (nên đặt trong /var/www/html/)

	mkdir /var/www/html/[tên thư mục]

- Tạo 1 trang web (file html thôi) trong thư mục vừa tạo

	vi /var/www/html/[tên thư mục vừa tạo phía trên]/[tên file]

> Lưu ý: có thể dùng lệnh sau để làm nhanh: 

> 	echo "thông điệp gì để hiện lên trang web thôi" >> /var/www/html/[tên thư mục vừa tạo phía trên]/[tên_file.html]


- Chỉnh file cấu hình tại /etc/httpd/conf/httpd.conf

	+ Dòng 276: #ServerName www.example.com:80
	
	Bỏ dấu *#* và sửa: ServerName ABC:80  	
	(với ABC là IP của máy CentOS thầy cho)

	+ Dòng 292:  DocumentRoot "/var/www/html"

	Sửa lại đường dẫn phù hợp tới thư mục vừa tạo phía trên:
	DocumentRoot "/var/www/html/[tên thư mục]"

	+ Dòng 317: Tương tự sửa  	<Directory "/var/www/html">
			Thành 		 <Directory "/var/www/html/[tên thư mục vừa tạo]">

	+ Dòng 402: Thêm phía sau là tên file .html vừa tạo phía trên
	mặc dịnh: 	DirectoryIndex index.html index.html.var
	sửa thành: 	DirectoryIndex index.html index.html.var  [tên file tạo ở trên.html] 

> 		Lưu ý; nếu tên file có sẵn trong mặc định thì không cần

- Khởi động lại dịch vụ 

	service httpd restart

- Cho dịch vụ khởi động cùng hệ thống

	chkconfig httpd on


# FTP 		(cần cài phần mềm *vsftpd*)=================================

- Chỉnh sửa file /etc/vsftpd/vsftpd.conf

	+ DÒng 12: Sử a YES thành NO

	+ Dòng 52, 62, 81, 82: Bỏ dấu *#*

- Set biến  ftp_home_dir = on

	setsebool -P ftp_home_dir 1

- Tạo user để test

	useradd [tên user]  		; Tạo user
	passwd  [tên user] 		; Đổi mật khẩu cho user

> Lưu ý: Có thể tạo file trong home user này nhanh như sau:

> 	echo "nội dung file ghi gì cũng đc" >> tên_file.txt

- Khởi động dịch vụ: 

	service vsftpd restart

- Khởi động cùng hệ thống

	chkconfig vsftpd on

# DNS 		(cần cài phần mềm **)

#		bind-9.8.2-0.17.rc1.el6_4.6.x86_64.rpm
#		bind-libs-9.8.2-0.17.rc1.el6_4.6.x86_64.rpm
#		bind-utils-9.8.2-0.17.rc1.el6_4.6.x86_64.rpm

- Sửa file /etc/named.conf (sửa 2 chỗ dòng 11 và dòng 17)

0 options {
              listen-on port 53 { 127.0.0.1; 20.0.0.10;}; # them IP server (máy linux hiện tại)
              listen-on-v6 port 53 { ::1; };
              directory       "/var/named";
              dump-file       "/var/named/data/cache_dump.db";
              statistics-file "/var/named/data/named_stats.txt";
              memstatistics-file "/var/named/data/named_mem_stats.txt";
              allow-query     { localhost; 10.0.0.0/8;}; # cho phep day mang co the truy van DNS
              #allow-tranfer {}; dia chi slave
              recursion yes;
              
              dnssec-enable yes;
              dnssec-validation yes;
              dnssec-lookaside auto;
              
              /* Path to ISC DLV key */
              bindkeys-file "/etc/named.iscdlv.key";
              
              managed-keys-directory "/var/named/dynamic";
      };


- Sửa file /etc/named.rfc1912.zones  (sửa 2 chỗ dòng 13, 15 và dòng 37, 39)


// named.rfc1912.zones:
//
// Provided by Red Hat caching-nameserver package
//
// ISC BIND named zone configuration for zones recommended by
// RFC 1912 section 4.1 : localhost TLDs and address zones
// and http://www.ietf.org/internet-drafts/draft-ietf-dnsop-default-local-zones-02.txt
// (c)2007 R W Franks
//
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//

zone "congtru.com" IN {   	# Thêm domain muốn đặt vào (vd: congtru.com)
        type master;
        file "phangiaithuan"; 	# đặt tên file zone thuận (đặt j cũng đc nhưng phải nhớ)
        allow-update { none; };
};

zone "localhost" IN {
        type master;
        file "named.localhost";
        allow-update { none; };
};

zone "1.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.ip6.arpa" IN {
        type master;
        file "named.loopback";
        allow-update { none; };
};

zone "1.0.0.127.in-addr.arpa" IN {
        type master;
        file "named.loopback";
        allow-update { none; };
};

zone "0.0.20.in-addr.arpa" IN {  		# sửa số 0 thành IP server (linux hiện tại) nhưng viết ngược (lấy 3 opt đầu của IP r mới viết ngược)
        type master;
        file "phangiainghich"; 		# đặt tên file zone nghịch (tên j cũng đc nhưng nhớ file)
        allow-update { none; };
};

- Sửa tên máy tính (có thể làm hoặc không nhưng làm cho dể cấu hình về sau)

	+ #hostname [tên máy] 		(Ở đây t đặt là CentoS về sau cấu hình thấy CentOS là tên máy, nhớ thay đổi đúng theo tên máy vừa đặt, lệnh này đổi ở session hiện tại)

	+ Mở file  /etc/sysconfig/network  và sử dòng HOSTNAME=[tên máy]


- Tạo và cấu hình file phangiaithuan 

	+ vi /var/named/phangiaithuan

> Lưu ý: file này không có đuôi con khỉ khô j hết, tạo xong thì sửa như phía dưới

$TTL 1D
@       IN SOA  CentOS.congtru.com. root.congtru.com. (
                                        0       ; serial
                                        1D      ; refresh
                                        1H      ; retry
                                        1W      ; expire
                                        3H )    ; minimum
        
        		IN      	NS      		CentOS.congtru.com.
       	 	IN      	MX      	10      	mail.congtru.com.
        		IN      	A       		20.0.0.10
CentOS  	IN      	A       		20.0.0.10
mail    		IN      	A       		20.0.0.10
www     	IN      	CNAME   	CentOS.congtru.com.

## trong đó chú ý: 

	+ CentOS: là tên máy vừa đặt trên nha

	+ congtru.com: là cái tên miền muốn đặt

	+ 20.0.0.10: là IP server (cái này thầy sẽ đổi)

	+ Chú ý có dấu chấm (.) ở cuối mấy cái tên miền (thiếu là sai)

- Tạo và cấu hình file phangiainghich

	+ vi /var/named/phangiainghich

> Lưu ý: file này không có đuôi con khỉ khô j hết, tạo xong thì sửa như phía dưới

$TTL 1D
@       IN SOA  CentOS.congtru.com. root.congtru.com. (
                                        0       ; serial
                                        1D      ; refresh
                                        1H      ; retry
                                        1W      ; expire
                                        3H )    ; minimum

        IN      NS      CentOS.congtru.com.
10      IN      PTR     CentOS.congtru.com.

## trong đó chú ý: 

	+ CentOS: là tên máy vừa đặt trên nha

	+ congtru.com: là cái tên miền muốn đặt

	+ Số 10 là số opt cuối cùng trong IP của server (ví dụ server linux của t là 20.0.0.10 nên t để là 10)

	+ Chú ý có dấu chấm (.) ở cuối mấy cái tên miền (thiếu là sai)

- Kiểm tra xem đã cấu hình đúng chưa sử dụng 3 lệnh sau

lệnh 1: named-checkconf 
lệnh 2: named-checkzone congtru.com /var/named/phangiaithuan 
lệnh 3: named-checkzone 0.0.20.in-addr.arpa /var/named/phangiainghich

## trong đó chú ý: 

	+ lệnh 1 là kiểm tra cấu hình file named.conf đã đúng chưa
	
	+ lệnh 2 là kiểm tra cấu hình file zone thuận đã đúng chưa

	+ lệnh 3 là kiểm tra cấu hình file zone nghịch đã đúng chưa

	+ Chú ý: congtru.con và 0.0.20.in-addr.arpa là 2 địa chỉ đã cấu hình ở trên

- Để chắc ăn thì nên phần quyền các file trên và thay đổi quyền sở hữu (k làm cũng đc)

# chmod 755 /etc/named.conf
# chmod 755 /etc/named.rfc1912.zones 
# chmod 755 /var/named/phangiaithuan 
# chmod 755 /var/named/phangiainghich 
# chown named:named /var/named/phangiaithuan 
# chown named:named /var/named/phangiainghich

- Khởi động lại dịch vụ

	service named restart 

- Khởi động cùng hệ thống

	chkconfig named on

> Lưu ý: đặt DNS bên client khi kiểm tra 



# MAIL 		(cần cài phần mềm *postfix* và *dovecot*)=======================

> Lưu ý: CentOS thường mặc định có sẵn cái dịch vụ để gửi mail r (tên là sendmail), vì vậy h tắt cái đó đi để dùng postfix mới cài.

lệnh 1: service sendmail stop 		(Không cho dịch vụ sendmail khởi động cùng hệ thống)
lệnh 2: chkconfig sendmail off  		(Chuyển mail agent từ sendmail sang postfix)
lệnh 3: alternatives --config mta 		(Chọn số nào ở dòng có postfix, sau đó enter)

> Lưu ý: nếu kiểm tra sendmail chưa cài thì không cần làm các bước trên

- Cấu hình file /etc/postfix/main.cf

	+ Dòng 77: Thêm dòng:  myhostname = mail.congtru.com

	> Điền cái mail server lúc nảy cấu hình bên zone thuận đó

	+ Dòng 83: Sửa: mydomain = congtru.com

	> Đổi tên domain đã đặt

	+ Dòng 99, 113: Bỏ dấu #

	+ Dòng 164: Thêm dấu phẩy và $mydomain ở cuối dòng

	+ Dòng 264: Thêm mạng của máy dùng mail server (ở đây là địa chỉ mạng của win XP)

	+ DÒng 419: Bỏ dấu #

- Khởi động lại postfix

	service postfix restart 

- Khởi động cùng hệ thống

	chkconfig postfix on

- Cấu hình dovecot ( 3 file )

Trong file  /etc/dovecot/dovecot.conf

	+ Dòng 20, 26: Bỏ dấu #

Trong file  /etc/dovecot/conf.d/10-mail.conf 

	+ Dòng 24: Bỏ dấu #

Trong file  /etc/dovecot/conf.d/10-auth.conf 

	+ Dòng 9: Bỏ dấu # và thay đổi yes thành no

	+ Dòng 97: Thêm login vào ở cuối

Trong file  /etc/dovecot/conf.d/10-master.conf

	+ Dòng 19, 22, 40, 43, 82: Bỏ dấu #

	+ Dòng 83, 84: Thêm vào postfix vào cuối dòng

- Khởi động lại dịch vụ

	service dovecot restart 

- Khởi động cùng hệ thống

	chkconfig dovecot on








