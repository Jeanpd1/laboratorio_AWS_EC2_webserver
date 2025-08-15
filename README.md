# Laboratório AWS: Explorando o Amazon EC2

Este projeto documenta a criação de um servidor web de teste na AWS utilizando uma instância EC2. O laboratório foi realizado de duas formas: através do console de gerenciamento e via CloudShell.

## Parte 1: Instância EC2 via Console AWS

* **Configurações Principais:**
    * **Nome:** webserver-seu-nome
    * **AMI:** Amazon Linux 2 AMI (HVM)
    * **Tipo:** t2.micro
    * **Par de Chaves:** parchave-seunome
    * **IP Público:** Será de acordo com a sua instância, ele é pego em EC2->Instâncias->Clique no nome da sua instância, no campo "Detalhes em "Endereço IPv4 público"

* **Print da Tela de Configuração:**
    **

* **Códigos Utilizados no "Dados de usuário":**
    ```bash
    #!/bin/bash
    yum -y install httpd
    systemctl enable httpd
    systemctl start httpd
    echo '<html><h1>Olá do seu servidor web!</h1></html>' > /var/www/html/index.html
    ```

* **Print do Servidor Web Funcionando:**
    **

## Parte 2: Instância EC2 via CloudShell

* **Configurações Principais:**
    * **Nome:** instancia-seunome
    * **Par de Chaves:** parchave-seunome
    * **IP Público:** Será de acordo com a sua instância, ele é pego em EC2->Instâncias->Clique no nome da sua instância, no campo "Detalhes em "Endereço IPv4 público"
    * **Grupo de segurança:** seunome-grupo

* **Comandos Utilizados no CloudShell:**
    ```bash
    # =================================================================================
    # SCRIPT FINAL PARA CRIAR INSTÂNCIA EC2 WEB SERVER VIA AWS CLOUDSHELL
    # =================================================================================
    #
    # INSTRUÇÕES:
    # 1. Altere a variável NOME_BASE na linha abaixo para o seu nome.
    # 2. Copie e cole o script inteiro no terminal do AWS CloudShell.
    # 3. Pressione Enter.
    #
    # =================================================================================

    # --- Passo 1: Definição de Variáveis (ÚNICA PARTE A SER EDITADA) ---
    NOME_BASE="seunome"

    # --- O restante do script usa a variável acima para criar nomes únicos ---
    NOME_INSTANCIA="instancia-${NOME_BASE}"
    NOME_GRUPO_SEGURANCA="${NOME_BASE}-grupo"
    # Garanta que este nome seja idêntico ao do par de chaves criado na Parte 1.
    NOME_PAR_CHAVE="parchave-${NOME_BASE}"

    # --- Passo 2: Criação do Grupo de Segurança (Firewall) ---
    # Tenta criar um grupo de segurança. Se já existir, não gera erro e continua.
    aws ec2 create-security-group --group-name ${NOME_GRUPO_SEGURANCA} --description "Grupo para permitir acesso web HTTP" || true

    # --- Passo 3: Liberação da Porta 80 (HTTP) ---
    # Adiciona a regra de acesso web ao grupo. Se já existir, não gera erro.
    aws ec2 authorize-security-group-ingress --group-name ${NOME_GRUPO_SEGURANCA} --protocol tcp --port 80 --cidr 0.0.0.0/0 || true

    # --- Passo 4: Lançamento da Instância EC2 ---
    # Cria a instância com todas as configurações. O script para instalar o servidor
    # web é passado diretamente, já codificado em Base64.
    aws ec2 run-instances \
        --image-id $(aws ssm get-parameters --names /aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64 --query 'Parameters[0].[Value]' --output text) \
        --instance-type t2.micro \
        --key-name ${NOME_PAR_CHAVE} \
        --security-groups ${NOME_GRUPO_SEGURANCA} \
        --tag-specifications "ResourceType=instance,Tags=[{Key=Name,Value='${NOME_INSTANCIA}'}]" \
        --user-data "IyEvYmluL2Jhc2gKeXVtIHVwZGF0ZSAteQp5dW0gaW5zdGFsbCAteSBodHRwZApzeXN0ZW1jdGwgc3RhcnQgaHR0cGQKc3lzdGVtY3RsIGVuYWJsZSBodHRwZAplY2hvICI8aHRtbD48aDE+U2V1IHNlZ3VuZG8gc2Vydmlkb3Igd2ViIGNyaWFkbyBwZWxvIENsb3VkU2hlbGw8L2gxPjwvaHRtbD4iID4gL3Zhci93d3cvaHRtbC9pbmRleC5odG1s"

    echo "================================================================================"
    echo "Comando enviado! Sua instância está sendo criada."
    echo "Acesse o console do EC2 para copiar o IP público e testar o acesso."
    echo "================================================================================"
    ```

* **Print do Servidor Web via CloudShell Funcionando:**
    **

## Encerramento

* **Print da tela com as instâncias sendo encerradas:**
    **

## Conclusão

Este laboratório proporcionou uma experiência prática e fundamental sobre o funcionamento do Amazon EC2. Aprendi a provisionar recursos, configurar grupos de segurança e automatizar tarefas com o CloudShell, competências essenciais para a computação em nuvem.
