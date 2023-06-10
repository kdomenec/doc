## Create key pair
```
$ gpg --expert --full-gen-key
   (1) RSA and RSA (default)
What keysize do you want? (3072) 4096
What keysize do you want for the subkey? (3072) 4096
         0 = key does not expire
Real name: Jean Gabin
Email address: jean@gabin.fr
Comment: C'est du vent le cinéma, de l'illusion, des bulles, du bidon.
```

## List keys
```
$ gpg --list-keys
$ gpg --list-secret-keys
```

## Import key pair
```
gpg --import --armor
gpg --import public.key
gpg --import private.key
```

## Export key pair
```
$ gpg --export --armor "Jean Gabin"
$ gpg --export-secret-key --armor "Jean Gabin"
```

## Delete key pair
```
$ gpg --delete-keys "Jean Gabin"
$ gpg --delete-secret-keys "Jean Gabin"
```

## Encrypt
```
gpg --encrypt --sign --armor -r jean@gabin.fr
gpg --no-default-keyring --keyring jgabin.key --trust-model always -earuser
```

## Uncrypt message
```
$ gpg -a -d; echo
```

## Trust a key
```
gpg --edit-key 'Jean Gabin'
gpg> trust
  5 = I trust ultimately
Your decision? 5
gpg> save
```

## Kill agent
```
gpgconf --kill gpg-agent
```

## readings
* https://www.esev.com/blog/post/2015-01-pgp-ssh-key-on-yubikey-neo/
* https://www.redhat.com/sysadmin/creating-gpg-keypairs
* https://coderwall.com/p/tx_1-g/gpg-change-email-for-key-in-pgp-key-servers
* https://www.howtogeek.com/427982/how-to-encrypt-and-decrypt-files-with-gpg-on-linux/
