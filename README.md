# infra-proxmox

Proxmox VE (Virtual Environment) é uma plataforma de virtualização open source que permite criar, gerenciar e manter ambientes virtuais de forma eficiente e flexível. Desenvolvido com foco em simplicidade e desempenho, o Proxmox combina duas tecnologias principais: KVM (Kernel-based Virtual Machine) para a virtualização completa e LXC (Linux Containers) para a virtualização leve de contêineres.

Uma das principais vantagens do Proxmox VE é a sua facilidade de uso — através de uma interface web intuitiva, os administradores podem criar e monitorar múltiplas máquinas virtuais e contêineres, além de gerenciar recursos do hardware de forma centralizada. Essa plataforma também oferece recursos avançados como alta disponibilidade, backup integrado, migração ao vivo e suporte a clusters, tornando-se uma escolha popular para ambientes de TI de diferentes tamanhos, desde pequenas empresas até grandes data centers.

Por ser de código aberto, o Proxmox VE é uma solução acessível, altamente personalizável e constantemente atualizada por uma comunidade ativa. Sua flexibilidade e robustez fazem dele uma excelente opção para quem deseja implementar uma infraestrutura virtualizada confiável e escalável.

## VMs vs. Linux Containers

### VMs versus Linux Containers**

Ao longo do tempo, diversas soluções de virtualização surgiram para otimizar o uso de recursos de hardware e melhorar a eficiência na gestão de ambientes de TI. As duas principais tecnologias são as Máquinas Virtuais (VMs) e os Contêineres Linux, cada uma com suas características, vantagens e limitações.

**Máquinas Virtuais (VMs)**  
As VMs são ambientes completos e isolados que simulam um hardware físico. Cada VM possui seu próprio sistema operacional completo (como Windows ou Linux), juntamente com aplicativos e configurações, executando sobre um hipervisor (como Proxmox, VMware ou Hyper-V). Isso oferece um alto grau de isolamento e compatibilidade, permitindo rodar diferentes sistemas operacionais na mesma máquina física. No entanto, por precisarem de um sistema operacional completo para cada instância, elas consomem mais recursos (CPU, memória e armazenamento) e levam mais tempo para serem inicializadas.

**Linux Containers (LXC)**  
Contêineres são uma forma de virtualização mais leve que compartilha o sistema operacional host. Eles isolam aplicações em ambientes separados, mas usam o mesmo kernel do sistema operacional subjacente. Isso faz com que os contêineres sejam menores, mais rápidos para criar e consumir menos recursos. São ideais para executar múltiplas aplicações de forma eficiente e em grande escala, especialmente em ambientes de DevOps, automação e hospedagem de serviços. No entanto, essa leveza significa que eles oferecem menor isolamento comparado às VMs e só podem rodar aplicações compatíveis com o kernel do sistema host.

**Quando usar cada um?**  
- Use VMs quando precisar de isolamento completo, compatibilidade com diferentes sistemas operacionais ou segurança reforçada.  
- Use Contêineres quando desejar alta performance, baixa sobrecarga e escalabilidade rápida para aplicações compatíveis com o sistema operacional do host.

## Instalação do Proxmox VE

A instalação do Proxmox VE é um processo relativamente simples e direto, projetado para que o usuário possa configurar a plataforma de virtualização de forma eficiente. A seguir, apresento um passo a passo básico:

**1. Requisitos de Hardware**  
Antes de iniciar, verifique se seu hardware atende aos requisitos mínimos, como CPU compatível com virtualização (Intel VT-x ou AMD-V), pelo menos 4 GB de RAM (quanto mais, melhor para ambientes maiores), armazenamento adequado (HD ou SSD) e uma conexão de rede confiável.

**2. Download da Imagem ISO**  
Acesse o site oficial do Proxmox (https://www.proxmox.com) e baixe a imagem ISO mais recente do Proxmox VE.

**3. Criação de Mídia de Instalação**  
Grave a ISO em um DVD ou crie uma unidade USB bootável usando ferramentas como Rufus ou Etcher.

**4. Boot pelo Mídia de Instalação**  
Insira a mídia no servidor ou computador, configure a BIOS para boot pelo dispositivo USB ou DVD e inicie o sistema.

**5. Processo de Instalação**  
Ao iniciar a instalação, siga as instruções na tela:  
- Aceite os termos de licença.  
- Selecione o disco onde o Proxmox será instalado.  
- Configure o fuso horário e as configurações regionais.  
- Crie uma senha de administrador (root) forte.  
- Defina o endereço IP, máscara de rede, gateway e DNS (configuração de rede).  

**6. Finalização e Acesso**  
Após concluir a instalação, reinicie o sistema e remova a mídia de instalação. Acesse a interface web do Proxmox pelo navegador, usando o endereço IP configurado (exemplo: https://seu-ip:8006). Faça login com as credenciais criadas durante a instalação.

**7. Configuração Inicial**  
Na interface web, você pode criar clusters, configurar armazenamento, criar VMs e containers, além de ajustar configurações de rede e segurança.

## Transferindo Arquivos ISO para o Proxmox VE

Após instalar o Proxmox VE, frequentemente é necessário adicionar imagens ISO de sistemas operacionais para criar máquinas virtuais. Aqui estão os passos comuns para transferir arquivos ISO para o seu servidor Proxmox:

### Opção 1: Acesso via Interface Web (GUI)

1. **Acesse a interface web do Proxmox**  
   Abra o navegador e digite o endereço IP do seu servidor Proxmox, por exemplo: `https://seu-ip:8006`.

2. **Faça login**  
   Use as credenciais de administrador criadas durante a instalação.

3. **Navegue até o armazenamento**  
   No menu à esquerda, clique no seu armazenamento (geralmente chamado de `local` ou outro nome configurado).

4. **Faça o upload do arquivo ISO**  
   - Clique na aba **"Conteúdo"**.
   - Clique em **"Upload"**.
   - Selecione o arquivo ISO do seu computador.
   - Clique em **"Start"** para iniciar o upload.

### Opção 2: Transferência via SCP (Secure Copy)

Se preferir usar a linha de comando, pode transferir os arquivos ISO usando **SCP**:

1. **Abra o terminal ou Prompt de Comando** no seu computador.

2. **Use o comando SCP** para copiar o arquivo:

```bash
scp caminho/do/arquivo.iso root@seu-ip:/var/lib/vz/template/iso/
```

*Exemplo:*

```bash
scp arquivo.iso root@192.168.1.100:/var/lib/vz/template/iso/
```

3. **Autentique-se** com a senha do usuário root do Proxmox.

4. Após a transferência, o arquivo ficará disponível na pasta `iso` do armazenamento padrão.


### Opção 3: Compartilhamento de Pasta (NFS, CIFS)

Se você tiver uma rede maior, também pode montar uma pasta compartilhada em rede (como NFS ou SMB) e copiar os arquivos ISO para lá, acessando-os posteriormente pelo Proxmox.


## Como criar uma VM (Máquina Virtual) no Proxmox VE

1. **Acesse a interface web**:  
   Vá em `https://seu-ip:8006` e faça login.

2. **Clique em "Create VM"**:  
   Inicia o assistente de criação.

3. **Configurações básicas**:
   - Nome da VM.
   - Seleção do arquivo ISO do sistema operacional (Windows ou Linux).
   - Configuração do disco rígido (tamanho, tipo).
   - Atribuição de CPU e RAM.
   - Configuração de rede.

4. **Revisar e criar**:
   - Confirme as configurações.
   - Clique em **"Finish"**.

5. **Instalação do sistema**:
   - Inicie a VM e acesse pelo console.
   - Complete a instalação do sistema operacional seguindo o procedimento padrão.


## Como criar um LXC (Contêiner Linux) no Proxmox VE

1. **Acesse a interface web**:  
   Vá em `https://seu-ip:8006` e faça login.

2. **Clique em "Create CT"**:  
   Para iniciar o assistente de criação.

3. **Configurações básicas**:
   - Nome do contêiner.
   - Selecione um template Linux disponível (como Ubuntu, Debian, CentOS).
   - Defina o ID numérico, CPU e memória.
   - Configure o armazenamento de disco.
   - Ajuste a rede (modo bridged, DHCP ou IP fixo).

4. **Finalizar e iniciar**:
   - Revise as configurações.
   - Clique em **"Finish"** para criar.
   - Para usar, selecione e clique em **"Start"**.
   - Acesse pelo console para configurar o sistema Linux do container.
