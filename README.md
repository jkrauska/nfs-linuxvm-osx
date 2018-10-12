# nfs-linuxvm-osx
Some notes on connecting to a linux vm running on an osx host

## Problem Statement:
As much as a like terminal based text editors, vscode and atom are pretty nifty.  Most of my desktoping is done in osx. Most of my servering is done in linux.  I want tight loops between code edits in osx land and compiles and running programs in linux land.  For prototyping most of my work is done on local linux vms running in virtual box or vmware fusion, and that's the nature of most of the solutions, but they could also be applied to accessing servers on the internet.


## Things tried:

- Native file sharing as built in to vm platform. \
-- Mouting host shared folders from osx in to vm \
found to be too slow for disk operations. (tried lots of optimizing)

- rmate-like functionality in vscode \
Plugin described here: https://marketplace.visualstudio.com/items?itemName=rafaelmaiolla.remote-vscode \
Unfortunately this only operates on a single file at a time, not entire projects/repos.

- sshfs via MacFuse
-- Used to be my preferred solution, but found it to be to crashy and would occaionally interefere when I wanted to adjust ssh sessions to linux land. (requires lots of additional software)


## Current solution:

- nfs-server on linux, nfs client on osx
-- current preferred solution \
Described well here: https://serverfault.com/questions/716350/mount-nfs-volume-on-ubuntu-linux-server-from-macos-client
-- I used the automouter method described and then ALSO created symlinks in my home dir to make it easier to reach the automount dirs without thinking or using finder.

## Notes:

I typically make my VMs operate in bridged mode.  For added security I create a second virtual nic on the vm that is 'Private To My Mac'.  This provides a dedicated /24 that I can use for my NFS export without worry about it be exposed on my local lan as when in bridged mode. It also means that my nfs mounts don't change when my IP addressing chages on the bridged interfaces.

I'm not using nfs security. I'd like to, but I haven't looked in to it, and I'm exporting/mounting on lan isolated to my machine.

I'm not using nfsv4.  Previous notes suggest it can crash osx.  Would like to try anyway. nfsv4 should be faster?
