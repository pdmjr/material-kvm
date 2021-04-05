# Material sobre KVM

Esta página descreve a instalação e utilização básica do KVM (*Kernel-based Virtual Machine*), cuja tecnologia tem mais de 10 anos e suporta virtualização completa para linux em hardware x86 (Intel VT ou AMD-V).  

O KVM tem código aberto e é capaz de virtualizar imagens não modificadas do linux ou windows, possibilitando criar um hardware virtualizado privado para cada VM com, por exemplo, uma placa de rede lógica e um disco virtual, dentre outros recursos. 

Toda a instalação e configuração apresentada neste material será no modo *headless* (sem interface gráfica), mais adequado para o ambiente de servidor de rede.

Para mais informações sobre o KVM: [site oficial](https://www.linux-kvm.org/page/Main_Page).

Esse material foi elaborado com base em: [Tutorial KVM](https://github.com/ismaelviih/github/blob/master/Tutorial.md)

## Verificando a compatibilidade do processador com a virtualização

- Verificando a quantidade de núcleos.

```
$ grep -Eoc '(vmx|svm)' /proc/cpuinfo
```

- Verificando o suporte do processador à virtualização que o kvm necessita.

```
$ sudo apt install -y cpu-checker
```

```
$ kvm-ok
```

- Instalação dos componentes de software.

```
$ sudo apt install -y qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virtinst virt-manager libguestfs-tools libosinfo-bin
```

- Verificando se a biblioteca libvirtd está em execução.

```
$ sudo systemctl is-active libvirtd
```

-  Recomenda-se a do seu usuário nos grupos "libvirt" e "kvm" para questões de gerenciamento.

```
$ sudo usermod -aG libvirt $USER
$ sudo usermod -aG kvm $USER
```

- Verificando quais sistemas operacionais são suportados pelo KVM.

```
$ osinfo-query os
```

- Instalação de uma VM Debian a partir do arquivo .iso.

```
$ mkdir vm-debian
$ cd vm-debian
$ wget https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-10.9.0-amd64-netinst.iso
```

```
$ sudo virt-install \
	--name debian-vm1 \
	--os-type linux \
	--os-variant generic \
	--ram=1024 \
	--vcpus=1  \
	--virt-type kvm \
	--disk path=/var/lib/libvirt/images/debian-vm1.qcow2,format=qcow2,bus=virtio,size=5 \
	--graphics none \
	--location=/vagrant/vm-debian/debian-10.9.0-amd64-netinst.iso \
	--extra-args="console=tty0 console=ttyS0,115200" \
	--check all=off
```
