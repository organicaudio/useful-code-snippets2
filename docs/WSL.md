# Linux Subsystem for Windows (WSL)

## WSL1 vs. WSL2

- WSL2 uses a real linux kernel. 
- WSL1 is just a linux api wrapper around the windows kernel.

```shell
# list installed wls with version 
wsl --list --verbose

# update wsl from wsl1 to wsl2
wsl --set-version Ubuntu-20.04 2

# delete wsl
wsl --unregister Ubuntu-20.04
```

# WORKAROUND: No Internet connection in WSL2

Run: `sudo bash -c 'echo "nameserver 8.8.8.8" > /etc/resolv.conf'` or add the command to you bashrc. 

# symlink kubeconfig

```
mkdir -p ~/.kube
ln -s /mnt/c/Users/__USER__/.kube/config ~/.kube/config
```

# Git under WSL

[Official doc](https://docs.microsoft.com/de-de/windows/wsl/tutorials/wsl-git)

Use the git credential manager:

```
git config --global credential.helper "/mnt/c/dev/Git/mingw64/libexec/git-core/git-credential-manager.exe"
```