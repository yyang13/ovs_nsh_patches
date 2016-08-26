ovs nsh patches
===============

patch file list
---------------
```
0001-ovs-vxlan-gpe-vxlan-extension-to-support-vxlan-gpe-t.patch
0002-ovs-nsh-support-push-and-pop-actions-for-vxlan-gpe-a.patch
0003-Add-userspace-dataplane-nsh-support-and-remove-push_.patch
0004-Fix-too-large-stack-frame-size.patch
0005-Ethernet-header-must-be-kept-in-VxLAN-gpe-eth-NSH-fo.patch
0006-Fix-VxLAN-gpe-Eth-NSH-issues.patch
0007-Fix-ovs-dpdk-issues.patch
```

How to apply them
-----------------
```
$ git clone https://github.com/yyang13/ovs_nsh_patches.git
$ git clone https://github.com/openvswitch/ovs.git
$ cd ovs
$ git reset --hard 7d433ae57ebb90cd68e8fa948a096f619ac4e2d8
$ cp ../ovs_nsh_patches/*.patch ./
$ git am *.patch
```

How to build
------------
```
$ ./boot.sh
$ ./configure --with-linux=/lib/modules/`uname -r`/build
$ make
```

How to build ovs DPDK
---------------------
```
$ pushd ../
$ wget http://fast.dpdk.org/rel/dpdk-2.2.0.tar.gz
$ tar xf dpdk-2.2.0.tar.gz
$ export DPDK_DIR=$(pwd)/dpdk-2.2.0
$ cd $DPDK_DIR
$ sed -i 's/CONFIG_RTE_BUILD_COMBINE_LIBS=n/CONFIG_RTE_BUILD_COMBINE_LIBS=y/g' config/common_linuxapp
$ make install T=x86_64-native-linuxapp-gcc DESTDIR=install
$ export DPDK_BUILD=$DPDK_DIR/x86_64-native-linuxapp-gcc/
$ popd
$ ./boot.sh
$ ./configure --with-dpdk=$DPDK_BUILD
$ make
```

If you want to overwrite local ovs installation, please also run it, but please be careful.

```
$ sudo make install
```

Test network topology
---------------------
![Test Network Topology](https://raw.githubusercontent.com/yyang13/ovs_nsh_patches/master/test-topo.png)

Openflow tables
---------------
- [test-flows-host1.txt](https://github.com/yyang13/ovs_nsh_patches/blob/master/test-flows-host1.txt)
- [test-flows-host2.txt](https://github.com/yyang13/ovs_nsh_patches/blob/master/test-flows-host2.txt)
