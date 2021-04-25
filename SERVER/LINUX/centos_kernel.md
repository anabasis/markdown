# yum update 커널 패닉

```bash
rpm -q kernel

kernel-4.18.0-193.el8.x86_64
kernel-4.18.0-240.22.1.el8_3.x86_64(패닉발생)

uname -r
4.18.0-193.el8.x86_64

yum remove -y kernel-4.18.0-240.22.1.el8_3.x86_64
```

yum update시에 커널 업데이트 제외

```bash
yum update --exclude=kernel*

vi /etc/yum.conf
exclude=kernel*
```

## grubby 사용법

```bash
## 설명
grubby --help

Usage: grubby [OPTION...]

Help options:
  -?, --help                          Show this help message
  --usage                             Display brief usage message
```

<table>
<tr><td>옵션</td><td>설명</td></tr>
<tr><td>--add-kernel=kernel-path</td><td>add an entry for the specified kernel</td></tr>
<tr><td>--add-multiboot=STRING</td><td>add an entry for the specified multiboot kernel</td></tr>
<tr><td>--args=args</td><td>default arguments for the new kernel or new arguments for kernel being updated</td></tr>
<tr><td>--mbargs=STRING</td><td>default arguments for the new multiboot kernel or new arguments for multiboot kernel being updated</td></tr>
<tr><td>--bad-image-okay</td><td>don`t sanity check images in boot entries (for testing only)</td></tr>
<tr><td>--boot-filesystem=bootfs</td><td>filesystem which contains /boot directory (for testing only) boot sector</td></tr>
<tr><td>--bootloader-probe</td><td>check which bootloader is installed on</td></tr>
<tr><td>-c, --config-file=path</td><td>path to grub config file to update ("-" for stdin)</td></tr>
<tr><td>--copy-default</td><td>use the default boot entry as a template for the new entry being added; if the default is not a linux image, or if the kernel referenced by the default image does not exist, the first linux entry whose kernel does exist is used as the template</td></tr>
<tr><td>--debug</td><td>print debugging information for failures</td></tr>
<tr><td>--default-kernel</td><td>display the path of the default kernel</td></tr>
<tr><td>--default-index</td><td>display the index of the default kernel</td></tr>
<tr><td>--default-title</td><td>display the title of the default kernel</td></tr>
<tr><td>--elilo</td><td>configure elilo bootloader</td></tr>
<tr><td>--efi</td><td>force grub2 stanzas to use efi</td></tr>
<tr><td>--env=path</td><td>path for environment data</td></tr>
<tr><td>--extlinux</td><td>configure extlinux bootloader (from syslinux)</td></tr>
<tr><td>--grub</td><td>configure grub bootloader</td></tr>
<tr><td>--grub2</td><td>configure grub2 bootloader</td></tr>
<tr><td>--info=kernel-path</td><td>display boot information for specified kernel</td></tr>
<tr><td>--initrd=initrd-path</td><td>initrd image for the new kernel</td></tr>
<tr><td>-i, --extra-initrd=initrd-path</td><td>auxiliary initrd image for things other than the new kernel</td></tr>
<tr><td>--lilo</td><td>configure lilo bootloader</td></tr>
<tr><td>--make-default</td><td>make the newly added entry the default boot entry</td></tr>
<tr><td>-o, --output-file=path</td><td>path to output updated config file ("-" for stdout)</td></tr>
<tr><td>--remove-args=STRING</td><td>remove kernel arguments</td></tr>
<tr><td>--remove-mbargs=STRING</td><td>remove multiboot kernel arguments</td></tr>
<tr><td>--remove-kernel=kernel-path</td><td>remove all entries for the specified
                                      kernel</td></tr>
<tr><td>--remove-multiboot=STRING</td><td>remove all entries for the specified
                                      multiboot kernel</td></tr>
<tr><td>--set-default=kernel-path</td><td>make the first entry referencing the
                                      specified kernel the default</td></tr>
<tr><td>--set-default-index=entry-index</td><td>make the given entry index the default
                                      entry</td></tr>
<tr><td>--set-index=entry-index</td><td>use the given index when creating a new
                                      entry</td></tr>
<tr><td>--silo</td><td>configure silo bootloader</td></tr>
<tr><td>--title=entry-title</td><td>title to use for the new kernel entry</td></tr>
<tr><td>--update-kernel=kernel-path</td><td>updated information for the specified
                                      kernel</td></tr>
<tr><td>-v, --version</td><td>print the version of this program and exit</td></tr>
<tr><td>--yaboot</td><td>configure yaboot bootloader</td></tr>
<tr><td>--zipl</td><td>configure zipl bootloader</td></tr>
</table>

```bash

## 정보조회
grubby --info=ALL

## 업데이트
grubby --update-kernel=ALL --args="spectre_v2=off nopti"

## 삭제
grubby --remove-kernel=kernel-path


```