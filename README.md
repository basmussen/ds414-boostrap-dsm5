# Synology DSM 5 - Install ipkg on a Synology DS414


### Install ipkg on a ds414 DSM 5

Actually there is no xsh bootstrap for the ds414 (Marvell Armada XP armv7l) although the existing Marvell Kirkwood mv6281 binaries "are ~ compatible" (http://ipkg.nslu2-linux.org/feeds/optware/cs08q1armel/). So this is a small guide to setup manually the optware environment, which based partly on [trepmag's guide]  (https://github.com/trepmag/ds213j-optware-bootstrap) - many thanks.


```
wget http://ipkg.nslu2-linux.org/feeds/optware/cs08q1armel/cross/stable/syno-mvkw-bootstrap_1.2-7_arm.xsh
chmod 700 syno-mvkw-bootstrap_1.2-7_arm.xsh
sh syno-mvkw-bootstrap_1.2-7_arm.xsh
```
Edit the bootstrap.sh file
```
vi bootstrap.sh
```
disable these lines by adding the # as line prefix
```
#if [ -e "$REAL_OPT_DIR" ] ; then
#    echo "Backup your configuration settings, then type:"
#    echo "  rm -rf $REAL_OPT_DIR"
#    echo "  rm -rf /usr/lib/ipkg"
#    echo "This will remove all existing optware packages."
#    echo
#    echo "You must *reboot* and then restart the bootstrap script."
#    exit 1
#fi

#if ! grep Feroceon-KW /proc/cpuinfo >/dev/null 2>&1; then
#    echo "Error: CPU not Marvell Kirkwood, probably wrong bootstrap.xsh"
#    exit 3
#fi
```

Run bootstrap
```
sh bootstrap.sh
```

Reboot your diskstation

```
/opt/bin/ipkg update
/opt/bin/ipkg list
/opt/bin/ipkg install pkg
```

Extend the PATH variable 'PATH=$PATH:/opt/bin:/opt/sbin' - check the [synology wiki] (http://forum.synology.com/wiki/index.php/Overview_on_modifying_the_Synology_Server,_bootstrap,_ipkg_etc)


# DSM 5 - Install Community python 2.7.x

Install [Community python 2.7.x] (http://www.synocommunity.com/). This python package include pycrypto module. 
[GitHub SynoCommunity] (https://github.com/SynoCommunity)

Set sym link.
```
ln -s /volume1/@appstore/python/bin/python /usr/bin/python
```

Run python
```
/usr/bin/python
Python 2.7.6 (default, Nov 11 2013, 13:32:18)
```

# DSM 5 - Install duplicity

```
ipkg update
/opt/bin/ipkg list | grep duplicity
ipkg install py26-duplicity
```


Run duplicity
```
/opt/bin/duplicity --version
```

### Solve errors


```
/opt/bin/duplicity --version
Segmentation fault (core dumped)
```

Edit duplicity
```
vi /opt/bin/duplicity
```

change interpreter from 
```
#!/opt/bin/python2.6
```
to 
```
#!/usr/bin/python
```

Run again:

```
/opt/bin/duplicity --version
```

Another error

```
Traceback (most recent call last):
  File "/opt/bin/duplicity", line 41, in <module>
    from duplicity import log
ImportError: No module named duplicity
```


Set PYTHONPATH environment variable to solve it

```
export PYTHONPATH=/opt/lib/python2.6/site-packages 
```

Finished, run duplicity again

```
./duplicity --version
duplicity 0.6.21
```

### Additional

Install py26-paramiko for ssh2 support (solve BackendException)
```
ipkg install py26-paramiko
```

Symlink
```
ln -s /opt/bin/gpg-agent /usr/syno/bin/gpg-agent
```


