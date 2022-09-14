# Manual para criar partições criptografadas em ambiente GNU/Linux
Um breve passo a passo de como criar partições encriptadas para proteger seus arquivos pessoais de acessos indevidos.
LVM on LUKS

# 1. Criando um container temporário para ser encriptado:
```
cryptsetup open --type plain /dev/sdb container -> Cria o container no disco /dev/sdb para listar o discos e selecionar o que você deseja utilize o comando fdisk -l
dd if=/dev/zero of=/dev/mapper/container -> preenchendo o disco com zeros (forma segura de formatar um disco), pode ser com dados aleatorios também.
dd if=/dev/urandom of/dev/sdb -> dados aleatórios
cryptsetup close container -> fechando o container
```

# 2. Criando o container definitivo que armazenará os dados:

```
- cryptsetup luksFormat /dev/sdb1
- cryptsetup open --type luks /dev/sdb1 container-destravado  
- pvcreate /dev/mapper/container-destravado -> Criando volume físico
- vgcreate nome-do-vg /dev/mapper/container-destravado -> Criando grupo lógico
- lvcreate -l +100%FREE nome-do-vg -n nomeDoLg -> Criando Volume lógico
- mkfs.ext4 /dev/mapper/nome--do--vg-nomeDoLg

```
# 3. Montando o disco manualmente para armazenar arquivos:

```
- cryptsetup open --type luks /dev/sdb1 container-destravado
- mount -t ext4 /dev/mapper/nome--do--vg-nomeDoLg /mnt/nomeDoLg/
```
### Desmontando

```

- umount /mnt/nomeDoLg/
- cryptsetup close container-destravado

```
# 3.1. Outra forma de montar e desmontar a partição:

- cryptsetup luksOpen /dev/sdb1 container
- cryptsetup luksClose container
- /sbin/mkfs.ext4 -c -m 1 -O dir_index,filetype,extent,sparse_super /dev/mapper/container
