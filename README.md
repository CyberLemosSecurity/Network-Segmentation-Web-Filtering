

1. Objetivo
   
   O objetivo deste projeto foi implementar um Laboratório de Borda (Edge) utilizando um firewall FortiGate para gerenciar o tráfego de uma rede interna (LAN) em direção à internet (WAN). O foco principal foi garantir que 100% do tráfego fosse inspecionado, eliminando "vazamentos" de rede e aplicando políticas de conformidade através de filtragem de conteúdo.

2. Environment

   A estrutura foi montada em ambiente virtualizado (VMware Player/Workstation), segmentando as interfaces da seguinte forma:

  Interface WAN (Port1): Configurada via DHCP para receber o link da operadora (VIVO).
  Interface LAN (Port2): Definida como Gateway da rede interna (10.0.1.254/24).
  Endpoints de Teste: Máquinas Windows e Ubuntu configuradas com IP estático e apontando para o FortiGate como saída padrão.

3. Analysis

   A Falha de Direcionamento: Inicialmente, a regra de firewall estava configurada como WAN -> WAN. Isso resultava em "navegação fantasma" (o PC navegava pelo Wi-Fi físico, não pelo firewall). A correção para LAN -> WAN foi o que permitiu a inspeção real.
Visibilidade via CLI: Utilizamos o comando diag sniffer packet port2 'none' 4 para validar o TCP Three-Way Handshake. Ver as flags SYN, SYN-ACK e ACK confirmou que o FortiGate estava processando o tráfego bidirecionalmente.

4. Data collection

Implementação de Web Filtering (Segurança de Conteúdo)
  Implementamos uma camada de segurança de aplicação (Camada 7) para controle de navegação.
  Perfil utilizado: Perfil Default customizado.
  Categoria Bloqueada: Social Networking (Redes Sociais).
  Inspeção SSL: Configurada como Certificate Inspection para identificar o SNI dos pacotes HTTPS.

<img width="936" height="621" alt="image" src="https://github.com/user-attachments/assets/8ce3b51e-92d4-476b-8832-419a5abb6b56" />

5. Conclusion


<img width="1485" height="264" alt="image" src="https://github.com/user-attachments/assets/299f2949-fc15-454a-b5b7-ca0fa39a9c95" />


<img width="736" height="686" alt="image" src="https://github.com/user-attachments/assets/211f91d7-0be9-4d1d-8a50-090483497405" />





















<img width="775" height="237" alt="image" src="https://github.com/user-attachments/assets/31f9dece-55ab-4238-8491-2ca6b6396061" />



