# Docker-CaraKernel-Termux
> a repository to help you use **Docker** in Termux with the "CaraKernel" kernel

---

### Requirements:
 - **CaraKernel Support** (I just tested it on device [#Ginkgo](https://github.com/AkariOficial/Docker-CaraKernel-Termux#this-step-would-be-the-compilation-of-the-kernel-but-not-necessarily-if-your-kernel-already-has-the-parameters-enabled-good-luck))
 - **Recovery** (Recommended [OrangeFox](https://orangefox.download/))
 - **Magisk/ROOT** ([v23+](https://github.com/topjohnwu/Magisk))

---

1. __First I rooted my Xiaomi Redmi Note 8. Then I installed [PixelOS](https://pixelos.net/download) Android 13.__
2. __Install [Termux](https://github.com/HardcodedCat/termux-monet). Then execute Moby’s script to check kernel’s compatibility o running docker.__

### Before we proceed <br> Check kernel compatibility
```bash
 pkg in wget tsu -y
 wget https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh
 chmod +x check-config.sh
 sed -i '1s_.*_#!/data/data/com.termux/files/usr/bin/bash_' check-config.sh
 sudo ./check-config.sh
```

--- 

3. __The missing configs will be displayed. Take notes of these red missing configs (especially configs under Generally Necessary), we have to enable them during kernel compliation.__
> ![Screenshot_Termux](https://user-images.githubusercontent.com/58480908/218159380-4b53280e-e049-4df7-a2ad-2ee46a8e8301.png)

---

### This step would be the compilation of the kernel. but not necessarily if your kernel already has the parameters [enabled](https://ivonblog.com/en-us/posts/sony-xperia-5-ii-docker-kernel/) (good luck)
 1. Before changing kernels, **backup dtbo and boot**
 2. **Download** [CaraKernel](https://t.me/GinkgoKernel/5804/40573?single) (for ginkgo)
 3. **Flash the ZIP to your** [#RECOVERY](https://github.com/AkariOficial/Docker-CaraKernel-Termux#requirements)
 4. **Flash the MAGISK (mandatory)**
   > **if it stays in bootloop, boot into recovery and restore the backup you made earlier.**

---

## It's show time...

> Running Docker containers
### A message “There is an internal problem with your device” will pop up on every boot. Just ignore it.

 1. **Open Termux, mount cgroups**:
 ```bash
  sudo mount -t tmpfs -o uid=0,gid=0,mode=0755 cgroup /sys/fs/cgroup
 ```
 2. **Enable binfmt_misc**:
 ```bash
  su
  mount binfmt_misc -t binfmt_misc /proc/sys/fs/binfmt_misc
  echo 1 > /proc/sys/fs/binfmt_misc/status
 ```
 3. Execute Moby’s script again: **sudo ./check-config.sh**. Make sure everything turns green.
 > ![Screenshot-Termux](https://user-images.githubusercontent.com/58480908/218163609-d6a5feeb-9477-43f4-83f1-83ed189f7a26.png) <br> Don't worry if it says: "**CONFIG_PID_NS: missing**, **CONFIG_IPC_NS: missing**, **CONFIG_CGROUP_DEVICE: missing**"

### Install docker and <br> docker-compose:
```bash
 pkg in root-repo -y
 pkg in docker docker-compose -y
```
---

### Start docker daemon (Swipe from left edge of the screen and open a new session. Run containers)
```bash
 sudo dockerd --iptables=false
```

### Assuming you've done what you asked above, let's try creating an **Ubuntu** container
```bash
 sudo docker run -it ubuntu bash
```
> **The similar result will be**:
> ![Screenshot_Termux](https://user-images.githubusercontent.com/58480908/218167294-2e31a558-9a79-4ff9-95f2-59d92fa551ab.png)

---

### “Maybe you're wondering why the "apt update" command doesn't work”, yes, I let it go unnoticed, because I want to show you how to fix it:
```bash
sudo docker run -ti \
    --net="host" \
    --dns="8.8.8.8" \
    ubuntu
```
> running this way, the apt command will work normally. <br> ps: **Don't forget to remove the container we created earlier** 
### So we can do this:
```bash
 sudo docker ps -a
 sudo docker rm <container_id>
```
> **something similar to this will be**:
> ![Screenshot_Termux](https://user-images.githubusercontent.com/58480908/218170437-03cbf2d2-9ad1-42f3-a1aa-877a71c5dc3d.jpg)


