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

## Backup & Snpshot

Os backups no Proxmox VE são essenciais para proteger suas máquinas virtuais (VMs) e contêineres (LXC), garantindo a recuperação rápida de dados em caso de falhas, erros ou necessidade de migração. Eles consistem em cópias completas ou incrementais dos discos e configuração do sistema, que podem ser armazenadas em diferentes locais, como armazenamento interno, externo ou na rede.

**Criando backups de VM e containers**  
Para criar backups no Proxmox, você acessa a interface web, seleciona a VM ou LXC desejada e inicia o processo de backup clicando em "Backup Now". Nesse momento, você escolhe o armazenamento de destino, o tipo de backup (completo ou diferencial) e confirma. O backup será realizado, armazenando a imagem do sistema, que poderá ser restaurada posteriormente.

**Restaurando backups**  
Para restaurar uma VM ou container, navegue até o armazenamento onde os backups estão guardados, selecione o arquivo desejado e clique em "Restaurar". Você pode configurar algumas opções, como um novo ID para a VM/Container, e então confirmar. Assim, a VM ou LXC será recuperada ao estado do backup escolhido.

**Agendando backups**  
Você pode programar backups automáticos acessando "Datacenter" > "Backup". Basta criar uma nova tarefa de backup, definir quais VMs ou containers devem ser incluídos, escolher o armazenamento de destino, o tipo de backup e definir a frequência (diária, semanal, etc.). Essa automatização garante que suas máquinas estejam sempre protegidas sem intervenção manual frequente.

### Snapshots no Proxmox VE

Snapshots capturam o estado exato de uma VM ou contêiner de forma instantânea, incluindo a memória, o disco, a configuração e o estado atual do sistema. Eles funcionam como uma “fotografia” que permite você retornar ao momento em que foi feito.

**Para que servem?**  
São úteis principalmente para testes, atualizações ou alterações temporárias. Você pode fazer um snapshot antes de fazer modificações importantes no sistema, testar configurações ou atualizações, e, se algo der errado, simplesmente reverter para o estado anterior.

**Como funcionam?**  
- O snapshot registra o estado do disco e da memória da VM ou do container no momento da criação.  
- Os snapshots são armazenados no mesmo espaço de armazenamento da VM ou container, o que pode consumir bastante espaço se feitos frequentemente ou com muitas alterações.

**Vantagens e limitações:**  
- **Vantagens:**  
  - Rápida recuperação do estado anterior.  
  - Permite testes sem risco de perda de dados.  
- **Limitações:**  
  - Se usados indiscriminadamente, podem consumir muita capacidade de armazenamento.  
  - Não são uma solução duradoura de backup, pois dependem do mesmo hardware do sistema principal.  

## Recursos avançados

### Criação de Templates de LXC no Proxmox VE

**O que são Templates de LXC?**  
Templates de LXC são imagens pré-configuradas de sistemas Linux que podem ser usadas para criar novos contêineres de forma rápida e padronizada. Eles facilitam a implantação eficiente de múltiplos containers com configurações similares, economizando tempo e garantindo consistência.

**Por que criar um template?**  
Criar um template personalizada permite usar uma configuração, sistema operacional, pacotes, configurações de rede e demais customizações específicas sempre que precisar criar um novo container, acelerando o processo de implantação.

#### Como criar um template de LXC no Proxmox

1. **Prepare um container base:**  
   - Crie um novo container (sem a necessidade de montar discos adicionais).  
   - Instale e configure o sistema Linux desejado, incluindo pacotes e configurações necessárias.  

2. **Apague dados específicos e configure o sistema:**  
   - Faça o container ficar limpo, removendo logs, chaves específicas, ou qualquer dado sensível que não deseja replicar.  
   - Certifique-se de que o sistema está estável e atualizado.

3. **Converta o container em template:**  
   - No Proxmox, vá até o armazenamento onde está o container.  
   - Localize o conteúdo do container na pasta `/var/lib/lxc/<ID>` ou na pasta correspondente ao armazenamento.  
   - Renomeie ou mova a pasta do container para a pasta de templates, geralmente `/var/lib/lxc/template/`, ou use a interface web para marcar como template (dependendo da versão).

4. **Utilize o template para criar novos containers:**  
   - Na interface web do Proxmox, ao criar um novo container, escolha seu template na lista de templates disponíveis.  
   - O sistema usará essa imagem para rapidamente gerar novos containers prontos para uso.

### Templates de VM no Proxmox VE

Templates de VM são cópias padronizadas e pré-configuradas de máquinas virtuais, que podem ser usadas como base para criar novas VMs de forma rápida e eficiente. Eles funcionam como uma imagem pronta com o sistema operacional e configurações já instaladas.

**Por que criar um template de VM?**  
Utilizar templates agiliza a implantação de múltiplas VMs similares, garantindo consistência na configuração, economia de tempo e redução do esforço de instalação manual.

#### Como criar um template de VM no Proxmox VE

1. **Criar uma VM base:**  
   - Crie uma VM normalmente, instalando o sistema operacional desejado e configurando-a conforme suas necessidades (instalações de software, configurações de rede, atualizações, etc.).

2. **Realizar ajustes finais:**  
   - Remova dados específicos, logs temporários ou qualquer configuração que não deva ser clonada.  
   - Desligue a VM após a configuração.

3. **Converter a VM em template:**  
   - No Proxmox, localize a VM que deseja transformar em template.  
   - Com a VM desligada, vá até a opção de editar ou configurar sua VM.  
   - Marque como **"Template"** (dependendo da versão do Proxmox, essa opção pode estar na interface de edição ou disponível ao clonar).  
   - Alternativamente, você pode fazer isso manualmente, acessando o armazenamento e renomeando ou movendo os discos para indicar que é um template.

4. **Usar o template para criar novas VMs:**  
   - Quando for criar uma nova VM, escolha o template na lista de templates disponíveis.  
   - O Proxmox irá clonar essa imagem, criando uma nova VM pronta para uso, com todas as configurações já aplicadas.


### Sistema de notificações no Proxmox VE

**1. Tipos de notificações**  
O Proxmox envia notificações sobre diversos eventos importantes, como:  
- Conclusão ou falha em backups  
- Avisos ou erros do sistema  
- Problemas no cluster  
- Alertas de hardware  
- Limites de uso sendo atingidos

**2. Como acessar as notificações**  
- No menu superior direito da interface web, há um ícone de sino (campainha) que indica novas notificações.  
- Ao clicar nele, você verá uma lista com as notificações recentes, incluindo detalhes e o nível de prioridade.

**3. Configurar notificações por email**  
Para receber alertas de forma proativa, você pode configurar o envio de emails:

- **Configurar o SMTP:**  
  - Vá em **Datacenter (Datacenter)** > **Options (Opções)** > **Email**.  
  - Insira os dados do seu servidor SMTP, endereço de email remetente e destinatário(s).  
  - Salve as configurações.

- **Testar envio de email:**  
  - Use a opção de enviar um email de teste para verificar se as configurações estão corretas.

**4. Gerenciar e filtrar notificações**  
- As notificações aparecem com diferentes níveis de severidade, como Informação, Aviso ou Crítico.  
- Você pode descartar ou marcar como lidas as notificações na própria tela de alertas.  
- Para ações mais avançadas, o ideal é usar ferramentas externas de monitoramento.

**5. Integração com ferramentas externas**  
- Para monitoramento mais completo, você pode integrar o Proxmox com sistemas como **Zabbix**, **Nagios** ou **Prometheus**, que fazem consultas na API do Proxmox para gerar alertas customizados e dashboards de controle.

### Storages no Proxmox VE

No Proxmox VE, **storage** é onde você armazena as imagens de discos, backups, templates, e outros dados relacionados às suas máquinas virtuais (VMs) e contêineres. É fundamental escolher e configurar bem os storages para garantir desempenho, segurança e facilidade de gerenciamento.


#### Opções de armazenamento disponíveis no Proxmox

1. **ZFS**  
   - Sistema de arquivos avançado com suporte a snapshots, compactação, replicação e integridade de dados.  
   - Pode ser configurado como pool de armazenamento direto na CPU ou em discos físicos.  
   - Ideal para armazenamento local com alta confiabilidade e recursos avançados.

2. **LVM (Logical Volume Manager)**  
   - Gerencia volumes lógicos sobre discos físicos, permitindo criar volumes flexíveis.  
   - Pode ser usado com ou sem suporte a snapshots, dependendo da implementação.  
   - Indicado para configurações que exijam alta performance e fácil expansão.

3. **Directory (Pasta Local)**  
   - Armazenamento baseado em diretórios no sistema de arquivos local (/var/lib/lxc, /var/lib/qemu, etc.).  
   - Simples e padrão, adequado para armazenamento de pequenos ambientes, testes ou backups locais.

4. **NFS (Network File System)**  
   - Compartilhamento de armazenamento via rede usando protocolo NFS.  
   - Permite usar armazenamento externo ou em rede, como NAS.  
   - Ideal para ambientes com múltiplos servidores Proxmox ou para armazenamento compartilhado.

5. **iSCSI**  
   - Protocolo de armazenamento em bloco via rede usando iSCSI.  
   - Permite conectar o Proxmox a storages SAN (Storage Area Network) de alta performance.  
   - Usado em ambientes empresariais para grandes volumes de dados.

6. **Ceph**  
   - Sistema de armazenamento distribuído e escalável.  
   - Integra-se nativamente ao Proxmox, oferecendo alta disponibilidade, replicação automática e desempenho em ambientes grandes.  
   - Recomendado para clusters de alta disponibilidade com grande volume de dados.

7. **GlusterFS**  
   - Sistema de arquivos distribuídos para armazenamento em rede.  
   - Pode ser configurado para fornecer armazenamento compartilhado escalável.

---

#### Como escolher o storage ideal?

- **Para ambientes de teste ou pequenos**: use armazenamento de diretório local ou NFS.  
- **Para alta disponibilidade e performance**: considere ZFS, Ceph ou iSCSI.  
- **Para armazenamento de grandes volumes de dados**: use Ceph ou iSCSI em ambientes de produção.  
- **Para facilidade de implementação**: NFS e diretórios locais são mais simples de configurar.

### Gerenciamento de Redes no Proxmox VE

O Proxmox VE oferece diversas opções para configurar e gerenciar redes, permitindo criar ambientes virtualizados com conectividade adequada às suas necessidades, seja para acesso externo, comunicação entre VMs, ou redes isoladas.

#### Principais opções de configuração de rede no Proxmox

1. **Bridge (Ponte de Rede)**  
   - É a forma mais comum de gerenciar rede no Proxmox.  
   - Funciona como uma ponte (bridge) que conecta as interfaces físicas do host às VMs e contêineres.  
   - Assim, as VMs podem receber IPs na mesma rede do host ou via DHCP, como se estivessem conectadas a uma switch física.  
   - Exemplo: `vmbr0` conectado à interface física `eth0`.

2. **Interfaces físicas (NICs)**  
   - Você configura uma ou mais interfaces físicas no host para se conectarem ao seu switch, roteador ou outro equipamento de rede.  
   - Essas interfaces podem ser usadas sozinhas ou como parte de bridges.

3. **VLANs (Virtual LANs)**  
   - Permitem segmentar a rede física em várias redes lógicas isoladas.  
   - Você pode configurar VLANs na interface física e criar bridges específicas para cada VLAN, permitindo VMs se comunicarem em VLANs separadas.

4. **IP Aliases e Redes Virtuais**  
   - Configuração de IPs adicionais no host, ou redes internas distintas, para diferentes ambientes ou serviços.

5. **Ratranqueamento de IPs / DHCP**  
   - Você pode configurar suas VMs para obter IPs via DHCP ou definir IPs estáticos nas configurações de rede de cada VM ou contêiner.

6. **Rede interna e isolada**  
   - Criar redes virtuais internas (sem conexão externa) usando bridges isolados ou isolando containers com redes internas para testes de segurança ou ambientes de desenvolvimento.

### Como gerenciar de forma prática

- **Configurar uma bridge**:  
  - No arquivo de configuração (`/etc/network/interfaces` no Debian/Proxmox), você define bridges conectados às interfaces físicas.  
  - Você também pode criar e editar via web, acessando o **Datacenter > Nodes > seu node > System > Network**.

- **Adicionar interfaces ou VLANs**:  
  - Pode fazer via interface gráfica ou manualmente editando os arquivos de configuração de rede.

- **Conectividade das VMs**:  
  - Vincule as interfaces virtuais das VMs às bridges, facilitando a comunicação com a rede física ou interna.

### Atualizações do Proxmox VE

O Proxmox VE é baseado em uma distribuição Debian, com seus próprios repositórios de pacotes específicos do Proxmox. Assim, as atualizações do sistema são feitas através do gerenciamento de pacotes do Debian, mas com foco nas melhorias, correções e novidades do próprio Proxmox.

#### Repositórios de atualização
- **Repositórios do Proxmox:** Contêm atualizações específicas para componentes do Proxmox, como o kernel, ferramentas de gerenciamento, APIs, etc.
- **Repositórios Debian:** Usados para atualizações de sistema operacional, pacotes gerais do Debian.


#### Atualização via linha de comando
1. **Abra o terminal** no Proxmox (via SSH ou console local).
2. **Atualize a lista de pacotes disponíveis:**
```bash
apt update
```
3. **Realize a atualização dos pacotes (inclui melhorias e correções):**
```bash
apt dist-upgrade
```
> O comando `dist-upgrade` também instala ou remove pacotes conforme necessário para completar as atualizações.

4. **Reinicie o sistema (se necessário):**  
Se o kernel foi atualizado, é recomendável reiniciar:
```bash
reboot
```

#### Atualizações pela interface web
- Acesse a interface gráfica do Proxmox (`https://seu-ip:8006`).
- Vá em **Datacenter** > seu nó > **Updates**.
- Clique em “Check for Updates” ou “Atualizar” para verificar se há novas versões e aplicar atualizações facilmente.

### Boas práticas para atualizações do Proxmox

1. **Backup completo:**  
   Antes de qualquer atualização importante, faça backup de suas VMs, contêineres e configurações do sistema.

2. **Verifique a compatibilidade:**  
   Leia as notas de versão (release notes) do Proxmox para entender possíveis mudanças que possam impactar seu ambiente.

3. **Atualize periodicamente:**  
   Faça atualizações frequentes para manter seu sistema protegido contra vulnerabilidades e receber melhorias.

4. **Teste em ambiente de testes:**  
   Se possível, teste as atualizações em um ambiente separado antes de aplicar no ambiente de produção.

5. **Realize atualizações durante janelas de manutenção:**  
   Programar atualizações em momentos de menor impacto evita problemas inesperados e indisponibilidade.

6. **Leia os avisos e notas de release:**  
   Algumas versões podem conter mudanças que exigem atenção especial, como configurações ou migrações de sistema.

## Migrações para o ProxMox

## Automação

## Outros