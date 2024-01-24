Задача: Есть приватная сеть, которая смотрит в интернет через NAT Mikrotik. Нужно заблокировать весь Bittorrent трафик из вне и изнутри сети.

Решение: 

Step 1: Go to IP > Firewall > Layer7 Protocols tab. Click on '+' to add a new entry.

![image](https://github.com/busuek/documentation_systemadmin/assets/101875725/2e2de429-c15d-43cf-80ae-edde48061154)

Step 2: Enter ‘torrent’ in the Name field. Copy and paste the following Perl expression in full in the Regexp field:

^(\x13bittorrent protocol|azver\x01$|get /scrape\?info_hash=get /announce\?info_hash=|get /client/bitcomet/|GET /data\?fid=)|d1:ad2:id20:|\x08'7P\)[RP]
Click on Comment to label the protocol entry as "Block Torrents". Click Apply then OK


>The MikroTik terminal command for adding the above rule is as follows:
>/ip firewall layer7-protocol add comment="Block Torrents" name=torrent regexp="^(\\x13bittorrent protocol|azver\\x01\$|get /scrape\\\?info_hash=get /announce\\\?info_hash=|get /client/bitcomet/|GET /data\\\?fid=)|d1:ad2:id20:|\\x08'7P\\)[RP]"

In Winbox, click on New Terminal. Copy and paste the above command in the terminal window and hit enter. The firewall rule as seen in the images above will be added instantly.

Step 3: Create filter rules to effectively block BitTorrent traffic.

Go to IP > Firewall > Filter Rules tab. Click on '+' to add a new entry.
In the General tab, select chain as forward, and select In Interface List as LAN

![image](https://github.com/busuek/documentation_systemadmin/assets/101875725/f53722d3-339c-43da-8298-4402e1bc6b52)

The purpose of defining In Interface List in this case is so that otherwise all P2P clients' remote IP addresses also get unnecessarily added to the 'Torrent-Conn' address list. Less addresses = less memory consumption

You can define a LAN list in the Interfaces > List section and add all local networks in the LAN list for identification purposes.

Step 4: In the Advanced tab, set the Src Address Listfield as "allow-bit", with the invert option "!" enabled.
select the Layer7 Protocol as torrent from the dropdown list.

![image](https://github.com/busuek/documentation_systemadmin/assets/101875725/aad13ba4-a467-4bf2-a7d2-ee5d5a8d188a)

Step 5: In the Action tab, select Action as add src to address list, and in the Address List field, enter "Torrent-Conn"

Set the Timeout as 00:00:30 (30 seconds)
Click on Comment to label the rule as "Block Torrents". Click Apply then OK

![image](https://github.com/busuek/documentation_systemadmin/assets/101875725/7706791d-4a4f-4a32-b821-a4fbb555f45f)

>The MikroTik terminal command for adding the above rule is as follows:
>/ip firewall filter add chain=forward in-interface-list=LAN src-address-list=!allow-bit layer7-protocol=torrent action=add-src-to-address-list address-list=Torrent-Conn address-list-timeout=30s comment="Block Torrents"

This filter rule will dynamically add host addresses on the local network to which the BitTorrent traffic is being forwarded to from the router, to an address list "Torrent-Conn".
There are two more filter rules we need to add to ensure that no other traffic are blocked other than torrents.

Go to IP > Firewall > Filter rules and add a new rule.

In the General tab, select protocol as tcp
In the Dst Port field, add the below port numbers or simply copy paste from here
0-1023,1723,5900,5800,3389,8728,8291,14147,5222,59905
Click on the invert [!] option as seen below.

![image](https://github.com/busuek/documentation_systemadmin/assets/101875725/7512262f-840d-4d53-b0ef-f4c9acd73f7b)

In the Advanced tab, select the Src Address List as Torrent-Conn from the dropdown list

![image](https://github.com/busuek/documentation_systemadmin/assets/101875725/d57ab9c4-8aa7-47b2-bfdc-3a52728f4b00)

In the Action tab, select Action as drop from the dropdown list
Click on Comment to label the rule as "Block Torrents". Click on Apply then OK.

![image](https://github.com/busuek/documentation_systemadmin/assets/101875725/3db37827-d4cb-411b-aad9-b4377ee9159a)

>The MikroTik terminal command for adding the above rule is as follows:
>/ip firewall filter add chain=forward protocol=tcp dst-port=!0-1023,1723,5900,5800,3389,8728,8291,14147,5222,59905 src-address-list=Torrent-Conn action=drop comment="Block Torrents"

Click on '+' add another new rule
In the General tab, select protocol as udp
in the Dst Port field, add the below ports or copy paste from below:
0-1023,1723,5900,5800,3389,8728,8291,14147,5222,59905
Click on the invert [!] option as seen below.

![image](https://github.com/busuek/documentation_systemadmin/assets/101875725/b01da3aa-2111-46f9-a081-6587a07adac6)

In the Advanced tab, select the Src Address List as Torrent-Conn from the dropdown list

![image](https://github.com/busuek/documentation_systemadmin/assets/101875725/bd294ed0-680c-4f37-8443-0dd78690d185)

In the Action tab, select Action as drop from the dropdown list
Click on Comment to label the rule as "Block Torrents". Click on Apply then OK.

![image](https://github.com/busuek/documentation_systemadmin/assets/101875725/d163d1a3-a5ed-432c-afef-3ff9d638273c)


>The MikroTik terminal command for adding the above rule is as follows:
>/ip firewall filter add chain=forward protocol=udp dst-port=!0-1023,1723,5900,5800,3389,8728,8291,14147,5222,59905 src-address-list=Torrent-Conn action=drop comment="Block Torrents"

These two filter rules will block tcp and udp packets respectively with BitTorrent data from being forwarded over non-common ports to those hosts (devices) listed in the Torrent-Conn address list, thereby effectively blocking Torrent downloads.

Drag all these three filter rules labeled as Block Torrents above the essential firewall filter rules.

![image](https://github.com/busuek/documentation_systemadmin/assets/101875725/1ccad7c8-38de-49f8-89c6-95fce8eecb53)

If there are multiple local networks created on the MikroTik router, the above rules created will block torrent downloads across all networks.












