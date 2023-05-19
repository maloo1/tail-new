# tail-new
Bash wrapper to monitor newly created logs in a directory where the new log does not replace the old log (so tail -F cannot be used) 

There are two versions of this script: 
## tail-new
A stand alone version that just takes 1 argument which is the path to the files plus the start of the name of the files. This works similar to if using a glob with the standard tail, but you cannot use globs with this version (as they will be expanded and then when restarting the tail command it wont be able to provide the more "generic" file path... 

To install this version do the following:
```
sudo curl -o /usr/bin/tail-new https://raw.githubusercontent.com/maloo1/tail-new/main/tail-new
sudo chmod 755 /usr/bin/tail-new
```

An example of this being used could be (note the missing glob):
```
tail-new /var/log/app/filter
```

## tail-new-wrapper
This is a version that is intended to be aliased so that globs can be used.\
This version should (mostly) work the same as the standard tail command and does the following:
- If the -e or --new-tail commandline option is passed it will invoke the "enhanced" tail functionality where the files specified with a glob are monitored and tail restarted if a new file is created that matches the glob
- Otherwise the original command line arguments are passed through to the standard tail command.
  
To install this version you will also need to setup an alias as follows:
```
sudo curl -o /usr/bin/tail-new-wrapper https://raw.githubusercontent.com/maloo1/tail-new/main/tail-new-wrapper
sudo chmod 755 /usr/bin/tail-new-wrapper
sudo cat << 'EOF' >/etc/profile.d/tail-new-wrapper.sh
alias tail='set -f; new-tail'
new-tail(){
    command /usr/bin/tail-new-wrapper "$@"
    set +f
}
EOF
```

An example of this being used could be (note the glob):
```
tail -e /var/log/app/filter*
```

## tail-new-wrapper without replacing tail
Alternatively this version could be installed so it does not replace the existing tail

To install this version you will also need to setup an alias as follows:
```
sudo curl -o /usr/bin/tail-new-wrapper https://raw.githubusercontent.com/maloo1/tail-new/main/tail-new-wrapper
sudo chmod 755 /usr/bin/tail-new-wrapper
sudo cat << 'EOF' >/etc/profile.d/tail-new-wrapper.sh
alias ntail='set -f; new-tail'
new-tail(){
    command /usr/bin/tail-new-wrapper -e "$@"
    set +f
}
EOF
```

An example of this being used could be (note the glob):
```
ntail /var/log/app/filter*
```
