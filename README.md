# bitbox-base-deps
This is a temporary repository to host binary dependencies for the [BitBox Base](https://github.com/digitalbitbox/bitbox-base) project, built for Linux ARMv8 cpu architecture, also known as aarch64 or arm64.

The binaries have been build on a RockPro64 board using Armbian 4.4.178.

## electrs
Electrum Server in Rust, used to serve Electrum wallet clients.  
https://github.com/romanz/electrs

Reason for not using official binaries: no binary releases available.

Build it yourself (as root user):
```
# install Rust
mkdir rust
cd rust
curl https://static.rust-lang.org/dist/rust-1.34.1-aarch64-unknown-linux-gnu.tar.gz -o rust.tar.gz

# must return 'rust.tar.gz: OK'
echo "0565e50dae58759a3a5287abd61b1a49dfc086c4d6acf2ce604fe1053f704e53 rust.tar.gz" | sha256sum -c -

tar --strip-components 1 -xzf rust.tar.gz
./install.sh

# install dependencies
apt install clang cmake

# compile electrs
git clone https://github.com/romanz/electrs
cd electrs
git checkout tags/v0.6.2
cargo build --release

# install electrs
cp ./target/release/electrs /usr/bin/
```


## c-lightning
Bitcoin Lightning Network client in C  
https://github.com/ElementsProject/lightning

Reason for not using official binaries: package on [clightning launchpad](https://launchpad.net/clightning) depends on `clib6` >= 2.25, while stable branch of Armbian is still on 2.24.

Build it yourself (as root user), using the PPA branch from Christian Decker:
```
# build c-lightning
git clone https://github.com/cdecker/lightning.git
cd lightning/
git checkout ppa
apt install debhelper pkg-config fakeroot dpkg-dev
dpkg-buildpackage -b -rfakeroot -us -uc

# install c-lightning
cd ..
dpkg -i lightningd_0.7.0-1_arm64.deb
```

