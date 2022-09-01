# Git Install
## 1. Install git
Go to https://git-scm.com/download/win

Install with default settings

## 2. Add credentials
Open Git

```sh
$ git config --global user.name "<username>"

$ git config --global user.email "<email>"
```
## 3. Generate ssh credentials
```sh
$ ssh-keygen -t ed25519 -C "<email>"
# ---- OR -------
$ ssh-keygen -t rsa -b 4096 -C "<email>"
```

**File to save key:**
> use default 
>
> `<your home folder>/.ssh/id_ed25519`

**Enter passphrase:**

**Enter passphrase again:**

## 3.5 Test if key exists
```sh
$ cat <your home folder>/.ssh/id_ed25519.pub
ssh-ed25519 AJBJjJBJBJANNK+AJSDN/JNS/aJNAS user@mail.com
```

## 4. Add key to github
Go to github.com > Top right > Settings > SSH and GPG keys > New SSH key

**Title:** 
> Auction Online Project

**Key type:**
> Authentication Key

**Key:**
> `cat <your home folder>/.ssh/id_ed25519.pub`
>
> Copy paste

**Warning:** do not share file containing `-----BEGIN OPENSSH PRIVATE KEY-----`

**Add SSH key**

## 5. Clone directory
Go to `https://github.com/KUOnlineAuction/frontend` > Code (Green Button) > SSH > Copy

Open Git

```sh
cd "<path>"
git clone "<paste here>"
```
**Add fingerprint ...something something..?**
> yes
```
cd frontend
```
