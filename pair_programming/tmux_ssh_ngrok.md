# Pair programming with ngrok + ssh + tmux + vim

From time to time you can be in need to consult your work with anyone else.
At this moment you will surely need to share some code, show the results of your
work, etc.

There are [plenty solutions](http://www.pairprogramwith.me) to do it, but in my
opinion the most useful and widely accessible method is to share tmux sessions
over SSH. Configuration is quite easy and in the end you get the full control
over your environment.

## OSX configuration

### SSH configuration
Firstly you need to disable possibility of authentication via password (we want
only to access via public key). To do it edit `/etc/sshd_conf` and set options
`PasswordAuthentication` and `ChallengeResponseAuthentication` to `no`.

You can do it using a simple `sed` script:
```
$ sudo sed -E -i.bak 's/^(#\s*)?(PasswordAuthentication|ChallengeResponseAuthentication).*$/\2 no/' /etc/ssh/sshd_config
```

To allow your partner to log in to your account you will need to add her/his key
to `~/.ssh/authorized_keys` file. To do it you can simply append the key to the
file:
```
$ cat partners_key.pub >> ~/.ssh/authorized_keys
```

You can also use the gem called `github_auth` to get the SSH key from GitHub:
```
$ gem install github-auth
$ gh-auth add --users=partners_github_login
```

You need to do it with every user you want to allow to your pair programming session.

### TMUX involved
We can define the command that will be launched every time someone attempts to
login to your account via SSH. If the command fails the SSH connection will be
interrupted. For our purpose we will set the command to `tmux attach -t pair` -
which means that logged in user will automaticaly be attached to a session called
"pair". If there is no such session the user will be disconnected.

To set the command edit file `~/.ssh/authorized_keys`:
```
command="/path/to/tmux attach -t pair" ssh-rsa YourPartnersSSHKey partners_nickname@partnershost
```

However you can do it easier using the `github-auth` gem while adding the key:
```
$ gh-auth add --users=partners_github_login --command="$( which tmux ) attach -s pair"
```

### NGROK tunnelling
Download [ngrok](https://ngrok.com/download), register yourself to the free plan
and run the `ngrok`:
```
$ ./ngrok tcp 22
```

`ngrok` will create a tunnel tat allows anyone who knows the address of the tunnel
to connect via TCP to port 22 (SSH). To allow your partner to connect to your
machine copy the address from the `Forwarding` line, for example:
```
Forwarding                    tcp://0.tcp.ngrok.io:53698 -> localhost:22
```
and launch tmux session
```
$ tmux new -s pair
```

Now your partner can login via SSH
```
ssh your_login@0.tcp.ngrok.io -p 53698
```
### Enjoy
The actions will happen simultainously on your and your partners console.
When you will end the tmux session then your partner will be automaticaly
disconnected.

Now you can use your favourite command tools/editors/etc.

## Linux configuration

The configuration in Linux is similar - only thing that can be different is the
location of the `sshd_config` (for example in `/etc/ssh/sshd_config`).
