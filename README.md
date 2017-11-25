# linux_privacy
Playing around with anti-forensic settings. Just some ideas.


## Disabling Gnome last used

**Problem**: Entries of recently used files
```bash
ultem@sturdy:~$ cat .local/share/recently-used.xbel | head -10
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
## Disable LibreOffice Recent Documents

**Problem**: Recent document list including thumbnails of document
```
item oor:path="/org.openoffice.Office.Histories/Histories/org.openoffice.Office.Histories:HistoryInfo['PickList']/ItemList"><node oor:name="file:///media/supersecret/document.odt" oor:op="replace"><prop oor:name="Title" oor:op="fuse"><value>document</value></prop><prop oor:name="Filter" oor:op="fuse"><value>writer8</value></prop><prop oor:name="Password" oor:op="fuse"><value></value></prop><prop oor:name="Thumbnail" oor:op="fuse"><value>iVBORw0KGgoAAAANSUhEUgAAALUAAAEACAIAAAB6QaCMAABgbklEQVR4nO29ed    BV1ZW4fS6JcVZUVAREUJFZEF8FmREEREVAnBITjZiYoackVZmq01Xp6q7uTqXSlequJJ12SDAmJnFCVJRBJgEFkUGZZ5mUGRVFsbnfU+f53f3t99yXK1wGQc/+49a55+xh7
```

Removing PickList entries
```bash
ultem@sturdy:~/.config/libreoffice/4/user$ sed -i '/'PickList'/d' ./registrymodifications.xcu 
```

Disable view of recent docs
```bash
<item oor:path="/org.openoffice.Office.Common/History"><prop oor:name="PickListSize" oor:op="fuse"><value>0</value></prop></item>
```
Afterward, opening documents will not create new `PickList` entries.
