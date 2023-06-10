https://www.howtoforge.com/how-to-install-puppet-server-and-agent-on-debian-11/

### Server
```
hostnamectl set-hostname master.puppet.house.moker.fr
wget https://apt.puppet.com/puppet7-release-bullseye.deb
dpkg -i puppet7-release-bullseye.deb
apt-get update
apt-get install puppetserver r10k
source /etc/profile.d/puppet-agent.sh
echo "export PATH=$PATH:/opt/puppetlabs/bin/" | tee -a ~/.bashrc
puppetserver -v
vim /etc/default/puppetserver
# - JAVA_ARGS="-Xms2g -Xmx2g"
# + JAVA_ARGS="-Xms1g -Xmx1g"
systemctl daemon-reload
systemctl enable --now puppetserver
systemctl status puppetserver
puppet config set server master.puppet.house.moker.fr
puppet config set server master.puppet.house.moker.fr --section main
puppet config set runinterval 1h --section main
puppet config set environment production --section server
puppet config set dns_alt_names master.puppet,master.puppet.house.moker.fr --section server
cat /etc/puppetlabs/puppet/puppet.conf
systemctl restart puppetserver
puppetserver ca list --all
puppetserver ca sign --certname desk.house.moker.fr
puppetserver ca list --all
puppetserver ca list-all
puppet module install puppet-r10k
puppet module install puppetlabs/stdlib
puppet module install puppetlabs/accounts
puppet module install puppetlabs/firewall
mkdir /etc/puppetlabs/r10k
r10k deploy environment -pv
git remote set-head origin production
```
```
$ puppet cert generate puppet.moker.fr --dns_alt_names=puppet,puppet.moker.fr,laptop.house.moker.fr
```

### Agent
```
hostnamectl set-hostname laptop.house.moker.fr
wget https://apt.puppet.com/puppet7-release-bullseye.deb
dpkg -i puppet7-release-bullseye.deb
apt-get update
apt-get install puppet-agent
/opt/puppetlabs/bin/puppet resource service puppet ensure=running enable=true
source /etc/profile.d/puppet-agent.sh
echo "export PATH=$PATH:/opt/puppetlabs/bin/" | tee -a ~/.bashrc
source ~/.bashrc
puppet config set server puppet.moker.fr --section agent
puppet config set masterport 50440 --section agent
puppet config set ca_server puppet.moker.fr --section agent
puppet config set ca_port 50440 --section agent
systemctl restart puppet
systemctl status puppet
puppet ssl bootstrap
puppet agent -t --noop
```

### Singing
```
puppetserver ca list --all
puppetserver ca sign --certname laptop.house.moker.fr
```

### Eyaml
```
mkdir /etc/puppetlabs/keys
/opt/puppetlabs/puppet/bin/eyaml createkeys \
 --pkcs7-private-key=/etc/puppetlabs/puppet/keys/private_key.pkcs7.pem \
 --pkcs7-public-key=/etc/puppetlabs/puppet/keys/public_key.pkcs7.pem
cd /etc/puppetlabs
chown -R puppet puppet
chmod 500 puppet
chmod 400 puppet/keys/*.pem
eyaml encrypt --pkcs7-public-key=/etc/puppetlabs/puppet/keys/public_key.pkcs7.pem  -s 'plop'
eyaml decrypt --pkcs7-public-key=/etc/puppetlabs/puppet/keys/public_key.pkcs7.pem  --stdin
```

### Dns altnames
```
systemctl stop puppetserver
rm -rf $(puppet config print ssldir)
rm -rf $(puppet config print cadir)
puppetserver ca setup --subject-alt-names $(hostname -f),puppet,puppet.moker.fr
systemctl start puppetserver
```

### Misc
```
cat << EOF >> /tmp/gpg_answers
%echo Generating a Puppet Hiera GPG Key
Key-Type: RSA
Key-Length: 4096
Subkey-Type: ELG-E
Subkey-Length: 4096
Name-Real: Hiera Data
Name-Comment: Hiera Data Encryption
Name-Email: puppet@$(hostname -d)
Expire-Date: 0
%no-ask-passphrase
# Do a commit here, so that we can later print "done" :-)
# %commit
# %echo done
EOF
```
```
mkdir /etc/puppetlabs/keys/gpg
chmod 600 /etc/puppetlabs/keys/gpg
gpg --batch --homedir /etc/puppetlabs/keys/gpg --gen-key /tmp/gpg_answers
```
