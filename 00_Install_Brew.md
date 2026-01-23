

# How to Install Homebrew on MAC M4 Pro?  
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### (Enter your macOS password when prompted. It may look like you are not typing anything, which is normal security behavior.)

### Copy Paste the below command. BTW, you will observe these commands as a output of 1st command. 
```
echo >> /Users/anishrana/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv zsh)"' >> /Users/anishrana/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv zsh)"
```

### Post checks! ==>  

```
brew help
```
```
man brew
```
==> 🍺 ==> ✔︎ 
