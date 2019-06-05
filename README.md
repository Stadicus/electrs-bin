# bitbox-base-deps
Temporary repository to host binary dependencies for the BitBox Base project, built for aarch64 ARM cpu Linux.

The binaries have been build on a RockPro64 board using Armbian 4.4.178.

## electrs
source repository:  
https://github.com/romanz/electrs

Build it yourself:
```
# install Rust
mkdir -p /usr/local/src/rust
cd /usr/local/src/rust
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
git checkout tags/v0.6.1
cargo build --release

# install electrs
cp /usr/local/src/rust/electrs/target/release/electrs /usr/bin/
```


## c-lightning
source repository:  
https://github.com/ElementsProject/lightning

Build it yourself, using the PPA branch from Christian Decker:
```
git clone https://github.com/cdecker/lightning.git
cd lightning/
git checkout ppa
sudo apt install debhelper pkg-config fakeroot dpkg-dev
sudo dpkg-buildpackage -b -rfakeroot -us -uc
```

The package `lightningd_0.7.0-1_arm64.deb` is written in the partent directory and can be installed with `dpkg -i lightningd_0.7.0-1_arm64.deb`.

