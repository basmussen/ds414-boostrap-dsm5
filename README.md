# Synology DSM 5 - Install ipkg on a Synology DS414


Install ipkg on a ds414 DSM 5

Actually there is no xsh bootstrap for the ds414 (Marvell Armada XP armv7l) although the existing Marvell Kirkwood mv6281 binaries "are ~ compatible" (http://ipkg.nslu2-linux.org/feeds/optware/cs08q1armel/). So this is a small guide to setup manually the optware environment (ipkg, PATH and init scripts).

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
ipkg update
ipkg list
ipkg install pkg
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


