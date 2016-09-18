# Netcat

## I. Introduction
Netcat est décrit dans son manuel ainsi : "[...] used for just about anything under the sun". Veritable couteau suisse par sa flexibilité, il est un outil incontournable.

## II. Fingerprint HTTP
```{r, engine='bash'}
aas@ubuntu:~$ echo "HEAD / HTTP/1.0" | nc abcde.fr 80
HTTP/1.1 400 Bad Request
Date: Sun, 18 Sep 2016 15:59:39 GMT
Server: **Apache/2.2.3 (CentOS)**
Connection: close
Content-Type: text/html; charset=iso-8859-1
```

## III. Shell à distance
Le fichier `/tmp/f` stock les commandes entrées à distance via netcat. Ces commandes sont executées dans un shell dans l'ordre fifo. Ce shell est affiché à l'utilisateur distant.
```{r, engine='bash'}
aas@ubuntu:~$ rm -f /tmp/f; mkfifo /tmp/f
aas@ubuntu:~$ cat /tmp/f | /bin/sh -i 2>&1 | nc -l 1234 > /tmp/f

```
Dans un deuxième shell :
```{r, engine='bash'}
aas@ubuntu:~$ nc localhost 1234
$ 
```

## IV. Envoi d'un mail
```{r, engine='bash'}
$ nc [-C] localhost 25 << EOF
HELO host.example.com
MAIL FROM:<user@host.example.com>
RCPT TO:<user2@host.example.com>
DATA
Body of email.
.
QUIT
EOF
```

## V. Port Scanning
On utilise les options `-z` (scan) et `-v` (verbose)
```{r, engine='bash'}
aas@ubuntu:~$ nc -zv localhost 21-25
nc: connect to localhost port 21 (tcp) failed: Connection refused
Connection to localhost 22 port [tcp/ssh] succeeded!
nc: connect to localhost port 23 (tcp) failed: Connection refused
nc: connect to localhost port 24 (tcp) failed: Connection refused
nc: connect to localhost port 25 (tcp) failed: Connection refused
```

## VI. Version Scanning
On utilise `-w` pour couper la connexion une fois la bannière récupérée (après 1 seconde).
```{r, engine='bash'}
aas@ubuntu:~$ nc -w 1 localhost 20-30
SSH-2.0-OpenSSH_7.2p2 Ubuntu-4ubuntu2.1
```

## VII. Transfert de fichiers
Dans un premier shell :
```{r, engine='bash'}
aas@ubuntu:~$ nc -l 1234 < vuln
```
Dans un second shell :
```{r, engine='bash'}
aas@ubuntu:~$ nc localhost 1234 < /tmp/vuln
```
