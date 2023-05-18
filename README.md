# tail-new
Bash wrapper to monitor newly created logs in a directory

There are two versions of this script: 
1) tail-new
A stand alone version that just takes 1 argument which is the path to the files plus the start of the name of the files. This works similar to if using a glob with the standard tail, but you cannot use globs with this version (as they will be expanded and then when restarting the tail command it wont be able to provide the more "generic" file path... 

2) tail-new-wrapper
This is a version that is intended to be aliased so that globs can be used.
This version should (mostly) work the same as the standard tail command and does the following:
  If the -e or --new-tail commandline option is passed it will invoke the "enhanced" tail functionality where the files specified with a glob are monitored and tail restarted if a new file is created that matches the glob
  Otherwise the original command line arguments are passed through to the standard tail command.
  
To install this version you will also need to setup an alias as follows:
sudo curl -o /usr/bin/tail-new-wrapper https://raw.githubusercontent.com/maloo1/tail-new/main/tail-new-wrapper
sudo chmod 755 /usr/bin/tail-new-wrapper
sudo echo "alias tail='set -f;new-tail';new-tail(){ command /usr/bin/tail-new-wrapper \"$@\";set +f;}" >/etc/profile.d/tail-new-wrapper.sh
