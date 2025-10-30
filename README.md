# bazaar

All sorts of Unix utilities.

[![ShellCheck](https://github.com/thomasleplus/bazaar/workflows/ShellCheck/badge.svg)](https://github.com/thomasleplus/bazaar/actions?query=workflow:"ShellCheck")

## [randy](randy)

Generates random strings of desired length using a chosen alphabet.

> [!WARN]
> This script does not use a cryptography-grade pseudo random number
> generator. It is a convenient way to generate random strings for
> example if you need dummy data. But do NOT use it for anything
> that requires good entropy.

For example to generate a 20-character alphanumeric string:

```shell
randy -n 20 -r '[:alnum:]'
```

To generate a 16-character hexadecimal string with
uppercase letters:

```shell
randy -n 16 -r '0-9A-F'
```

To generate a UUID:

```shell
s="$(randy -n 32 -r '0-9a-f')"; printf "%s-%s-%s-%s-%s\n" "${s:0:8}" "${s:8:4}" "${s:12:4}" "${s:16:4}" "${s:20:12}"
```

To generate a fake email address:

```shell
s="$(randy -n 24 -r 'a-z')"; printf "%s.%s@%s.com\n" "${s:0:8}" "${s:8:8}" "${s:16:8}"
```

To generate a fake US phone number:

```shell
s="$(randy -n 10 -r '0-9')"; printf "(%s) %s-%s\n" "${s:0:3}" "${s:3:3}" "${s:6:4}"
```

See `randy -h` for details.

## [derdp](derdp) and [rerdp](rerdp)

Microsoft Remote Desktop Protocol files (`.rdp`) use an esoteric
encoding called `USC-2 LE BOM`. It makes it difficult to edit this
files with many text editors or with the usual command lines
utilities. The scripts `derdp` and `rerdp` allow convert back and
forth `USC-2 LE BOM` and the more traditional `latin1` encoding to
make things easier.

For example if you want to use `sed` to modify the content of a `.rdp`
file, you could do something like:

```shell
cat old.rdp | derdp | sed -e 's/foo/bar/g' | redrp > new.rdp
```

## [connectivity-check](connectivity-check)

### Overview

This script mimics the internet connectivity checks of various operating systems (iOS, Android, macOS, Windows, Linux...). Just run the script and it will start fetching various test pages.

If your network access is not working (Wi-Fi disconnected, problem with the Ethernet cable or modem router), you will see error messages like `curl: (7) Couldn't connect to server`.

If you have internet access, the HTTP status codes of the responses will be from the 2XX Success class (e.g. 200 OK or 204 No Content).

> Note that if the URLs ending in `generate_204` do not return a 204 HTTP status code, or if you see error messages like `curl: (6) Could not resolve host`, your connection might be blocked by a captive portal using [DNS Redirection](https://en.wikipedia.org/wiki/Captive_portal#Redirect_by_DNS) instead of [HTTP redirection](https://en.wikipedia.org/wiki/Captive_portal#HTTP_redirect).

If you have network connectivity but you do not have internet access because, for example, you have not yet signed into some sort of captive portal, you will probably get some sort of 3XX Redirection HTTP status code (e.g. 307 Temporary Redirect or 302 Found) followed by the URL of the captive portal.

Finally if you get a 4XX or 5XX HTTP error status code, it probably means that the corresponding requested URL is not valid anymore (or maybe the service is having some temporary issues).

### Samples

This is the program's output when internet is accessible:

```shell
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

```shell
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

## [open-clipboard](open-clipboard)

Reads a file path or URL from the clipboard and opens it in the corresponding system default application. I have this command associated to a custom keyboard shortcut (Ctrl+B). This is particularly useful and easy to remember when you want to open a non-clickable URL: just select the URL text and do Ctrl+C then Ctrl+B to open in default browser.

## [pip-audit](pip-audit)

Lists all pip-installed packages along with their versions, last access times, and installation paths. This is useful for identifying unused Python packages that can be safely removed.

```shell
pip-audit
```

You can specify which Python interpreter to use:

```shell
PYTHON=python3.12 pip-audit
```

> [!NOTE]
> Access times may be inaccurate if your filesystem uses the `noatime` mount option.

## [git-squash-unpushed-commits-with-oldest-message](git-squash-unpushed-commits-with-oldest-message)

Squashes all unpushed commits on the current branch into a single commit, keeping the message from the oldest commit. This is useful for cleaning up a feature branch before merging.

```shell
git-squash-unpushed-commits-with-oldest-message
```

The script automatically detects the upstream branch and only squashes commits that haven't been pushed yet.

## [git-squash-unpushed-commits-with-same-message](git-squash-unpushed-commits-with-same-message)

Squashes consecutive unpushed commits that have identical commit messages. This is useful when you've made multiple commits with the same message and want to combine them.

```shell
git-squash-unpushed-commits-with-same-message
```

## [github-create-labels](github-create-labels)

Creates a standardized set of Conventional Commits labels in GitHub repositories. This adds labels for `build`, `chore`, `ci`, `docs`, `feat`, `fix`, `perf`, `refactor`, `style`, `test`, and `no-release-notes`.

```shell
github-create-labels owner/repo1 owner/repo2
```

Requires the GitHub CLI (`gh`) to be installed and authenticated.

## [github-list-repos](github-list-repos)

Lists all Git clone URLs for repositories belonging to specified GitHub users. This includes both personal repositories and organization repositories the user belongs to.

```shell
github-list-repos username1 username2
```

Output can be piped to other commands for bulk operations:

```shell
github-list-repos myusername | xargs -n1 git clone
```

## [hdd-smart-info](hdd-smart-info)

Securely wipes hard drives and runs comprehensive SMART diagnostics. This script performs a two-pass random data wipe, executes a long SMART self-test, and generates a detailed report file named after the drive's serial number.

> [!WARNING]
> This script will PERMANENTLY DESTROY ALL DATA on the specified drives. Use with extreme caution.

```shell
sudo hdd-smart-info /dev/sda /dev/sdb
```

The script generates a report file `<serial-number>.txt` containing the SMART test results and drive information.
