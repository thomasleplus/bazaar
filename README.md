# bazaar

All sorts of unix utilities

[![ShellCheck](https://github.com/thomasleplus/bazaar/workflows/ShellCheck/badge.svg)](https://github.com/thomasleplus/bazaar/actions?query=workflow:"ShellCheck")

## [connectivity-check](connectivity-check)

### Overview

This script mimics the internet connectivity checks of various operating systems (iOS, Android, Mac OS, Windows, Linux...). Just run the script and it will start fetching various test pages.

If your network access is not working (Wi-Fi disconnected, problem with the ethernet cable or modem router), you will see error messages like `curl: (7) Couldn't connect to server`.

If you have internet access, the HTTP status codes of the responses will be from the 2XX Success class (e.g. 200 OK or 204 No Content).

> Note that if the URLs ending in `generate_204` do not return a 204 HTTP status code, or if you see error messagges like `curl: (6) Could not resolve host`, your connection might be blocked by a captive portal using [DNS Redirection](https://en.wikipedia.org/wiki/Captive_portal#Redirect_by_DNS) instead of [HTTP redirection](https://en.wikipedia.org/wiki/Captive_portal#HTTP_redirect). You can confirm this by looking to the responses to the SSL URLs.

If you have network connectivity but you do not have internet access because, for example, you have not yet signed into some sort of captive portal, you will probably get some sort of 3XX Redirection HTTP status code (e.g. 307 Temporary Redirect or 302 Found) followed by the URL of the captive portal.

Finally if you get a 4XX or 5XX HTTP error status code, it probably means that the corresponding requested URL is not valid anymore (or maybe the service is having some temporary issues).

### Samples

This is the program's output when internet is accessible:

```
$ connectivity-check
GET http://connectivity-check.ubuntu.com
HTTP/1.1 204 No Content

GET http://www.neverssl.com
HTTP/1.1 200 OK

GET http://www.msftconnecttest.com/connecttest.txt
HTTP/1.1 200 OK

GET http://www.msftncsi.com/ncsi.txt
HTTP/1.1 200 OK

GET http://clients3.google.com/generate_204
HTTP/1.1 204 No Content

GET http://connectivitycheck.android.com/generate_204
HTTP/1.1 204 No Content

GET http://connectivitycheck.gstatic.com/generate_204
HTTP/1.1 204 No Content

GET http://captive.apple.com/hotspot-detect.html
HTTP/1.1 200 OK

GET http://www.apple.com/library/test/success.html
HTTP/1.1 200 OK

GET https://www.google.com
HTTP/1.1 200 OK

GET https://www.apple.com
HTTP/1.1 200 OK
```

And this is what the output looks like when the internet access is being blocked by a captive portal doing DNS spoofing:

```
$ connectivity-check
GET http://connectivity-check.ubuntu.com
curl: (52) Empty reply from server

GET http://www.neverssl.com
curl: (52) Empty reply from server

GET http://www.msftconnecttest.com/connecttest.txt
curl: (52) Empty reply from server

GET http://www.msftncsi.com/ncsi.txt
curl: (52) Empty reply from server

GET http://clients3.google.com/generate_204
curl: (52) Empty reply from server

GET http://connectivitycheck.android.com/generate_204
curl: (52) Empty reply from server

GET http://connectivitycheck.gstatic.com/generate_204
curl: (52) Empty reply from server

GET http://captive.apple.com/hotspot-detect.html
curl: (52) Empty reply from server

GET http://www.apple.com/library/test/success.html
curl: (52) Empty reply from server

GET https://www.google.com
curl: (51) SSL: no alternative certificate subject name matches target host name 'www.google.com'

GET https://www.apple.com
curl: (51) SSL: no alternative certificate subject name matches target host name 'www.apple.com'
```

https://rootsh3ll.com/captive-portal-guide/

## [open-clipboard](open-clipboard)

Reads a file path or URL from the clipboard and opens it in the corresponding system default application. I have this command associated to a custom keyboard shortcut (Ctrl+B). This is particularly useful and easy to remember when you want to open a non-clickable URL: just select the URL text and do Ctrl+C then Ctrl+B to open in default browser.

## [random-string](random-string)

Generates random strings of desired length using a chosen alphabet.

For example to generate a 20-character alphanumeric string:

```
random-string -n 20 -r '[:alnum:]'
```

To generate a 16-character hexadecimal string with
uppercase letters:

```
random-string -n 16 -r '0-9A-F'
```

To generate a UUID:

```
u="$(random-string -n 32 -r '0-9a-f')"; printf "%s-%s-%s-%s-%s" "${u:0:8}" "${u:8:4}" "${u:12:4}" "${u:16:4}" "${u:20:12}"
```

See `random-string -h` for details.