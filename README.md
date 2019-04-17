<img src="resources/logo.png" width="300">

[![Build Status](https://travis-ci.org/ZerataX/matrix-registration.svg?branch=master)](https://travis-ci.org/ZerataX/matrix-registration) [![Coverage Status](https://coveralls.io/repos/github/ZerataX/matrix-registration/badge.svg)](https://coveralls.io/github/ZerataX/matrix-registration) ![PyPI - Python Version](https://img.shields.io/pypi/pyversions/matrix-registration.svg) [![PyPI](https://img.shields.io/pypi/v/matrix-registration.svg)](https://pypi.org/project/matrix-registration/) [![Matrix Chat](https://img.shields.io/badge/chat-%23matrix--registration%3Admnd.sh-brightgreen.svg)](https://matrix.to/#/#matrix-registration:dmnd.sh)
# matrix-registration

a simple python application to have a token based matrix registration

if you like me encountered the situation where you want to invite your friends to your homeserver, but neither wanted to open up public registration nor create accounts for every individual user yourself, this project should be the solution.

with this project you can just quickly generate tokens on the fly and share them with your friends to allow them to register to your homeserver.

## setup
```bash
pip3 install matrix_registration
```
Then download the [config.sample.yaml](config.sample.yaml) and save a modified version to `config.yaml` in the folder that you want to execute matrix-registration.

### nginx reverse-proxy
an example nginx setup:
```nginx
location  ~ ^/(static|register) {
        proxy_pass http://localhost:5000;
}
```

If you already have a website and want to use your own register page, the [wiki](https://github.com/ZerataX/matrix-registration/wiki/reverse-proxy#advanced) describes a more advanced nginx setup.


## usage
```bash
$ python -m matrix_registration -h
usage: python -m matrix_registration [-h] {api,gen,status} ...

a token based matrix registration app

positional arguments:
  {api,gen,status}  sub-commands. for ex. 'gen -h' for additional help
    api             start as api
    gen             generate new token. -o onetime, -e expire date
    status          view status or disable token. -s status, -d disable, -l
                    list

optional arguments:
  -h, --help
```

after you've started the api server and [generated a token](https://github.com/ZerataX/matrix-registration/wiki/api#creating-a-new-token) you can register an account with a simple post request, e.g.:
```bash
curl -X POST \
     -F 'username=test' \
     -F 'password=verysecure' \
     -F 'confirm=verysecure' \
     -F 'token=DoubleWizardSki' \
     http://localhost:5000/register
```
or a simple html form, see the sample [resources/example.html](resources/example.html)

the html page looks for the query paramater `token` and sets the token input field to it's value. this would allow you to directly share links with the token included, e.g.:
`https://homeserver.tld/register.html?token=DoubleWizardSki`


For more info check the [wiki](https://github.com/ZerataX/matrix-registration/wiki)
