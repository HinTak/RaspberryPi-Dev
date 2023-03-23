https://kojipkgs.fedoraproject.org//vol/fedora_koji_archive01/packages/kernel/4.4.14/200.fc22/x86_64/kernel-core-4.4.14-200.fc22.x86_64.rpm
https://kojipkgs.fedoraproject.org//vol/fedora_koji_archive01/packages/kernel/4.4.14/200.fc22/x86_64/kernel-devel-4.4.14-200.fc22.x86_64.rpm
https://kojipkgs.fedoraproject.org//vol/fedora_koji_archive01/packages/kernel/4.5.7/202.fc23/x86_64/kernel-core-4.5.7-202.fc23.x86_64.rpm
https://kojipkgs.fedoraproject.org//vol/fedora_koji_archive01/packages/kernel/4.5.7/202.fc23/x86_64/kernel-devel-4.5.7-202.fc23.x86_64.rpm
https://kojipkgs.fedoraproject.org//vol/fedora_koji_archive01/packages/kernel/4.8.16/200.fc24/x86_64/kernel-core-4.8.16-200.fc24.x86_64.rpm
https://kojipkgs.fedoraproject.org//vol/fedora_koji_archive01/packages/kernel/4.8.16/200.fc24/x86_64/kernel-devel-4.8.16-200.fc24.x86_64.rpm
https://kojipkgs.fedoraproject.org//vol/fedora_koji_archive01/packages/kernel/4.9.17/100.fc24/x86_64/kernel-core-4.9.17-100.fc24.x86_64.rpm
https://kojipkgs.fedoraproject.org//vol/fedora_koji_archive01/packages/kernel/4.9.17/100.fc24/x86_64/kernel-devel-4.9.17-100.fc24.x86_64.rpm
https://kojipkgs.fedoraproject.org//vol/fedora_koji_archive02/packages/kernel/4.19.16/200.fc28/x86_64/kernel-core-4.19.16-200.fc28.x86_64.rpm
https://kojipkgs.fedoraproject.org//vol/fedora_koji_archive02/packages/kernel/4.19.16/200.fc28/x86_64/kernel-devel-4.19.16-200.fc28.x86_64.rpm 

mkdir /tmp/t
cd /tmp/t

rpm2cpio ~/kojipkgs.fedoraproject.org/vol/fedora_koji_archive01/packages/kernel/4.4.14/200.fc22/x86_64/kernel-core-4.4.14-200.fc22.x86_64.rpm | cpio -m -d --extract
rpm2cpio ~/kojipkgs.fedoraproject.org/vol/fedora_koji_archive01/packages/kernel/4.4.14/200.fc22/x86_64/kernel-devel-4.4.14-200.fc22.x86_64.rpm | cpio -m -d --extract
...

ln -s /tmp/t/lib/modules/* /lib/modules/
ln -s /tmp/t/usr/src/kernels/* /usr/src/kernels/
