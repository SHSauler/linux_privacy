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

## Geany recently used
**Problem**: Text editor Geany uses file `/home/<user>/.config/geany/geany.conf` to store 10 (default) recently used files in `[files] recent_files=`.

Disable by adding `mru_length=0` to `/home/<user>/.config/geany/geany.conf`'s `[geany]` section. Existing recent file entries are removed.

## Reviews betray software installed

On Ubuntu-based distros, there's file `/home/<user>/.local/share/gnome-software/ubuntu-reviews.db`, a sqlite3 database that contains the user's reviews of software. 

It contains removed software packages as well. In my case `leafpad`.

```
sqlite> .tables
review_stats  reviews       timestamps  
sqlite> .schema review_stats
CREATE TABLE review_stats (package_name TEXT PRIMARY KEY,one_star_count INTEGER DEFAULT 0,two_star_count INTEGER DEFAULT 0,three_star_count INTEGER DEFAULT 0,four_star_count INTEGER DEFAULT 0,five_star_count INTEGER DEFAULT 0);
sqlite> select * from review_stats where package_name like "leafpad";
leafpad|3|3|8|10|39
```
## Transmission saves stats

**Problem:** No big deal, but Transmission saves stats about downloaded and uploaded files in `/home/<user>/.config/transmission/stats.json`. 

Setting all to zero and making the file unwritable seems to work.

```
ultem@sturdy:~/.config/transmission$ cat stats.json 
{
    "downloaded-bytes": 0,
    "files-added": 0,
    "seconds-active": 0,
    "session-count": 0,
    "uploaded-bytes": 0
}
ultem@sturdy:~/.config/transmission$ sudo chattr +i stats.json 
```
