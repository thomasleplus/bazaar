# bazaar
All sorts of unix utilities

## connectivity-check

This script mimics the internet connectivity checks of various operating systems (iOS, Android, Mac OS, Windows, Linux...). Just run the script and it will start fetching various test pages.

If your network access is not working (Wi-Fi disconnected, problem with the ethernet cable or modem router), you will see error messages like this one:
```
curl: (7) Couldn't connect to server
```

If you have internet access, the HTTP status codes of the responses will be from the 2XX Success class (e.g. 200 OK or 204 No Content).

If you have network connectivity but you do not have internet access because, for example, you have not yet signed into some sort of captive portal, you will probably get some sort of 3XX Redirection HTTP status code (e.g. 307 Temporary Redirect or 302 Found) followed by the URL of the captive portal.

Finally if you get a 4XX or 5XX HTTP error status code, it probably means that the corresponding requested URL is not valid anymore (or maybe the service is having some temporary issues).

* https://en.wikipedia.org/wiki/List_of_HTTP_status_codes
* https://rootsh3ll.com/captive-portal-guide/
