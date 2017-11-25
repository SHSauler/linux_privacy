# linux_privacy
Playing around with anti-forensic settings. Just some ideas.


## Disabling Gnome last used
```bash
user@sturdy:~$ cat .local/share/recently-used.xbel | head -10
<?xml version="1.0" encoding="UTF-8"?>
<xbel version="1.0"
      xmlns:bookmark="http://www.freedesktop.org/standards/desktop-bookmarks"
      xmlns:mime="http://www.freedesktop.org/standards/shared-mime-info"
>
```

Disable: 
```bash
ultem@sturdy:~$ echo "" > ~/.local/share/recently-used.xbel
ultem@sturdy:~$ echo !$
echo ~/.local/share/recently-used.xbel
/home/ultem/.local/share/recently-used.xbel
ultem@sturdy:~$ chattr +i !$
chattr +i ~/.local/share/recently-used.xbel
chattr: Operation not permitted while setting flags on /home/ultem/.local/share/recently-used.xbel
ultem@sturdy:~$ sudo !!
sudo chattr +i ~/.local/share/recently-used.xbel
ultem@sturdy:~$ echo "YAY" > !$
echo "YAY" > ~/.local/share/recently-used.xbel
bash: /home/ultem/.local/share/recently-used.xbel: Operation not permitted
```
