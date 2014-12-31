debian_deploy
=============

Automatic Debian installation via preseed.cfg and prepared netinstall.iso.


##Manual##

### Preseed ###
following https://www.debian.org/releases/stable/i386/apbs04.html.en

### Modify ISO ###
following https://wiki.debian.org/DebianInstaller/Preseed/EditIso

####Copy ISO for modification####
* mkdir loopdir
* mount -o loop debian-jessie-DI-b2-amd64-netinst.iso loopdir
* mkdir myiso
* rsync -a -H --exclude=TRANS.TBL loopdir/ cd
* umount loopdir

####Copy modified preseed.cfg####
* mkdir irmod
* cd irmod
* gzip -d < ../myiso/install.amd/initrd.gz | cpio --extract --verbose --make-directories --no-absolute-filenames
* cp ../preseed.cfg preseed.cfg
* sudo su
* find . | cpio -H newc --create --verbose | gzip -9 > ../myiso/install.amd/initrd.gz
* cd ../
* rm -fr irmod/```

####Checksum####
* cd myiso
* md5sum `find -follow -type f` > md5sum.txt
* cd ..

####Create ISO####
* genisoimage -o netinst.iso -r -J -no-emul-boot -boot-load-size 4 -boot-info-table -b isolinux/isolinux.bin -c isolinux/boot.cat ./myiso
