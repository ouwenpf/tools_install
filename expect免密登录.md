# 常用账户权限
```
#!/bin/bash

echo "start ..... "
hosts="172.16.0.8 172.16.0.15"
passwd='Gameads@2023'


if ! rpm -qa |grep expect &>/dev/null ;then
	yum install -y expect
fi

ssh-keygen -t rsa -N '' -f $HOME/.ssh/id_rsa -q
cat  $HOME/.ssh/id_rsa.pub  > $HOME/.ssh/authorized_keys 
chmod 600 $HOME/.ssh/authorized_keys 





for i in $hosts
do
echo "...............$host.................."
echo ''  $HOME/.ssh/known_hosts &>/dev/null
expect <<EOF

	spawn ssh-copy-id -f -p52553 $USER@$i
	expect {
		"yes/no" { send "yes\n"; exp_continue }
		"password" { send "$passwd\n" }
		
	
	}
	expect eof

EOF
done 





for k in $hosts
do
echo "...............$host.................."
echo ''  $HOME/.ssh/known_hosts &>/dev/null
expect <<EOF

	spawn  scp -P52553  -rp $HOME/.ssh/id_rsa  $HOME/.ssh/id_rsa.pub   $USER@$k:$HOME/.ssh 
	expect {
		"yes/no" { send "yes\n"; exp_continue }
		"password" { send "$passwd\n" }
		
	
	}
	expect eof

EOF
done 





```