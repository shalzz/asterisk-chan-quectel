You can add this repo as a package to openwrt via a custom feed:

```
src-link quectel /path/to/asterisk-chan-quectel/feed
```

You can then build the package with `$ make package/asterisk-chan-quectel/compile V=sc`
after selecting the package in menuconfig.
