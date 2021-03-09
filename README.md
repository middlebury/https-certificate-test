# https-certificate-test

`https-certificate-test` is a bash script to test that a webserver at an IP
address is properly configured with HTTPS certificates for many hostnames,
without having to add entries to your `/etc/hosts` file and checking with a
browser.

This command loops through a list of hostnames and makes a CURL request to
each one to validate that the webserver is properly configured to use the right
certificate for each hostname and that the certificates are valid.

It uses CURL's `--resolve` option to ensure that the request is going to the
webserver IP that you specify, but making the request for the hostname
specified:

```
curl -I -sS --resolve "${HOSTNAME}:443:${IP_ADDRESS}" "https://${HOSTNAME}/"
```

## Usage

```
./https-certificate-test <ip-address> <hostname_1> [... <hostname_n>]
```

or pipe hostnames via stdin:

```
echo "<hostname_1>
...
<hostname_n>" | ./https-certificate-test <ip-address>
```

Pass `-h` or `--help` to see usage.

## Examples

Test to see if an IP address will respond to a few hostnames:

```
./https-certificate-test 140.233.2.84 go.middlebury.edu go.miis.edu www.middlebury.edu www.miis.edu

go.middlebury.edu                                           OK
go.miis.edu                                                 OK
www.middlebury.edu                                          ERROR
curl: (60) SSL: no alternative certificate subject name matches target host name
'www.middlebury.edu' More details here: https://curl.se/docs/sslcerts.html curl
failed to verify the legitimacy of the server and therefore could not establish
a secure connection to it. To learn more about this situation and how to fix it,
please visit the web page mentioned above.

www.miis.edu                                                OK
```

Or, pass a file with hostnames on each line to the command:

```
cat hostnames.txt | ./https-certificate-test 140.233.2.84

go.middlebury.edu                                           OK
go.miis.edu                                                 OK
www.middlebury.edu                                          ERROR
curl: (60) SSL: no alternative certificate subject name matches target host name
'www.middlebury.edu' More details here: https://curl.se/docs/sslcerts.html curl
failed to verify the legitimacy of the server and therefore could not establish
a secure connection to it. To learn more about this situation and how to fix it,
please visit the web page mentioned above.

www.miis.edu                                                OK
```

# About
* **Repository:** https://github.com/middlebury/https-certificate-test
* **Author:** Adam Franco https://github.com/adamfranco
* **License:** GNU General Public License version 3 or later
