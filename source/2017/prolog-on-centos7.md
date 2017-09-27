---
title: 'Prolog on CentOS 7'
date: 2017-09-26
tags: [prolog, sysadmin]
---

## Install Prolog

```bash
# dependencies (see http://www.swi-prolog.org/build/Redhat.html)
# for me only autoconf was missing
sudo yum install -y autoconf

# clone swi-prolog repo
git clone https://github.com/SWI-Prolog/swipl-devel.git
cd swipl-devel
# run prepare script
./prepare --yes
# copy build script template
cp -p build.templ build
# run build script
./build
```

## Usage
To run a prolog program use `swipl test.pl` or just enter the repl with `swipl` and use

```prolog
% loads test.pl
[test].
% to exit repl
% press ctrl + c, type 'e' and enter
```
