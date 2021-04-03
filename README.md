# Material sobre KVM

Esta página descreve a instalação e utilização básica do KVM (*Kernel-based Virtual Machine*), cuja tecnologia tem mais de 10 anos e suporta virtualização completa para linux em hardware x86 (Intel VT ou AMD-V).  

O KVM tem código aberto e é capaz de virtualizar imagens não modificadas do linux ou windows, possibilitando criar um hardware virtualizado privado para cada VM com, por exemplo, uma placa de rede lógica e um disco virtual, dentre outros recursos. 

Toda a instalação e configuração apresentada neste material será no modo *headless* (sem interface gráfica), mais adequado para o ambiente de servidor de rede.

Para mais informações sobre o KVM: [site oficial](https://www.linux-kvm.org/page/Main_Page).

## Verificando a compatibilidade do processador com a virtualização

- Verificando a quantidade de núcleos.

```
grep -Eoc '(vmx|svm)' /proc/cpuinfo
```

- Verificando o suporte do processador à virtualização que o kvm necessita.

```
sudo apt install cpu-checker
```

```
kvm-ok
```
