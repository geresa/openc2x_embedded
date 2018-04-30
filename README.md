# OpenC2X embedded

To build the firmware:

* Update the feeds:
```
./scripts/feeds update -a
./scripts/feeds install -a
```
* Create the configuration, allowed architectures are x86_64, x86_geode, ar71xx_generic:
```
./create_config.sh x86_64
make defconfig
```
* Build
```
make
```
