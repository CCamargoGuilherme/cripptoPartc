# Manual para criar partições criptografadas em ambiente GNU/Linux
Um breve passo a passo de como criar partições encriptadas para proteger seus arquivos pessoais de acessos indevidos.
LVM on LUKS

# cryptsetup open --type plain /dev/sdb container
# dd if=/dev/zero of=/dev/mapper/container
# cryptsetup close container

# cryptsetup luksFormat /dev/sdb1
# cryptsetup open --type luks /dev/sdb1 container-destravado  
# pvcreate /dev/mapper/container-destravado
# vgcreate nome-do-vg /dev/mapper/container-destravado
# lvcreate -l +100%FREE nome-do-vg -n nomeDoLg
# mkfs.ext4 /dev/mapper/nome--do--vg-nomeDoLg

# cryptsetup open --type luks /dev/sdb1 container-destravado
# mount -t ext4 /dev/mapper/nome--do--vg-nomeDoLg /mnt/nomeDoLg/
# umount /mnt/nomeDoLg/
# cryptsetup close container-destravado



ou

cryptsetup luksOpen /dev/sdb1 container
/sbin/mkfs.ext4 -c -m 1 -O dir_index,filetype,extent,sparse_super /dev/mapper/container
