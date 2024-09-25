### Português

# Implement Load Balancing on Compute Engine: Challenge Lab

Este guia passo a passo ajudará você a configurar o balanceamento de carga no Google Cloud usando comandos `gcloud` e alternativas pelo console.

## Variáveis de Ambiente

Defina as variáveis de ambiente para facilitar a execução dos comandos:

```bash
export INSTANCE=<nome-da-instancia>
export FIREWALL=<nome-da-regra-de-firewall>
export ZONE=<sua-zona>
export REGION=<sua-região>

```

## 1. Criar Instância de VM

### Comando `gcloud`

```bash
gcloud compute instances create $INSTANCE \
    --zone=$ZONE \
    --machine-type=e2-micro

```

### Pelo Console

1.  Vá para Compute Engine > Instâncias de VM.
2.  Clique em “Criar Instância”.
3.  Configure a instância conforme necessário e clique em “Criar”.

## 2. Script de Inicialização

Crie um script de inicialização para instalar e configurar o Nginx:

```bash
cat << EOF > startup.sh
#! /bin/bash
apt-get update
apt-get install -y nginx
service nginx start
sed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.html
EOF

```

## 3. Criar Template de Instância

### Comando `gcloud`

```bash
gcloud compute instance-templates create web-server-template \
    --metadata-from-file startup-script=startup.sh \
    --machine-type g1-small \
    --region $REGION

```

### Pelo Console

1.  Vá para Compute Engine > Templates de Instância.
2.  Clique em “Criar Template de Instância”.
3.  Adicione o script de inicialização na seção “Script de inicialização”.
4.  Configure o restante conforme necessário e clique em “Criar”.

## 4. Criar Grupo de Instâncias Gerenciadas

### Comando `gcloud`

```bash
gcloud compute instance-groups managed create web-server-group \
    --base-instance-name web-server \
    --size 2 \
    --template web-server-template \
    --region $REGION

```

### Pelo Console

1.  Vá para Compute Engine > Grupos de Instâncias.
2.  Clique em “Criar Grupo de Instâncias”.
3.  Selecione o template de instância criado anteriormente.
4.  Configure o restante conforme necessário e clique em “Criar”.

## 5. Criar Regra de Firewall

### Comando `gcloud`

```bash
gcloud compute firewall-rules create $FIREWALL \
    --allow tcp:80 \
    --network default

```

### Pelo Console

1.  Vá para VPC Network > Regras de Firewall.
2.  Clique em “Criar Regra de Firewall”.
3.  Configure a regra para permitir tráfego TCP na porta 80.
4.  Clique em “Criar”.

## 6. Criar Verificação de Saúde HTTP

### Comando `gcloud`

```bash
gcloud compute http-health-checks create http-basic-check

```

### Pelo Console

1.  Vá para Network Services > Health Checks.
2.  Clique em “Criar Verificação de Saúde”.
3.  Configure a verificação de saúde HTTP e clique em “Criar”.

## 7. Configurar Portas Nomeadas

### Comando `gcloud`

```bash
gcloud compute instance-groups managed \
    set-named-ports web-server-group \
    --named-ports http:80 \
    --region $REGION

```

### Pelo Console

1.  Vá para Compute Engine > Grupos de Instâncias.
2.  Selecione o grupo de instâncias gerenciadas.
3.  Clique em “Editar” e adicione a porta nomeada `http:80`.
4.  Clique em “Salvar”.

## 8. Criar Serviço de Backend

### Comando `gcloud`

```bash
gcloud compute backend-services create web-server-backend \
    --protocol HTTP \
    --http-health-checks http-basic-check \
    --global

```

### Pelo Console

1.  Vá para Network Services > Backend Services.
2.  Clique em “Criar Serviço de Backend”.
3.  Configure o serviço de backend para usar o grupo de instâncias gerenciadas e a verificação de saúde.
4.  Clique em “Criar”.

## 9. Adicionar Backend ao Serviço de Backend

### Comando `gcloud`

```bash
gcloud compute backend-services add-backend web-server-backend \
    --instance-group web-server-group \
    --instance-group-region $REGION \
    --global

```

### Pelo Console

1.  Vá para Network Services > Backend Services.
2.  Selecione o serviço de backend criado.
3.  Adicione o grupo de instâncias gerenciadas como backend.
4.  Clique em “Salvar”.

## 10. Criar Mapa de URL

### Comando `gcloud`

```bash
gcloud compute url-maps create web-server-map \
    --default-service web-server-backend

```

### Pelo Console

1.  Vá para Network Services > URL Maps.
2.  Clique em “Criar Mapa de URL”.
3.  Configure o mapa de URL para usar o serviço de backend.
4.  Clique em “Criar”.

## 11. Criar Proxy HTTP

### Comando `gcloud`

```bash
gcloud compute target-http-proxies create http-lb-proxy \
    --url-map web-server-map

```

### Pelo Console

1.  Vá para Network Services > Target Proxies.
2.  Clique em “Criar Proxy HTTP”.
3.  Configure o proxy para usar o mapa de URL.
4.  Clique em “Criar”.

## 12. Criar Regra de Encaminhamento

### Comando `gcloud`

```bash
gcloud compute forwarding-rules create http-content-rule \
    --global \
    --target-http-proxy http-lb-proxy \
    --ports 80

```

### Pelo Console

1.  Vá para Network Services > Forwarding Rules.
2.  Clique em “Criar Regra de Encaminhamento”.
3.  Configure a regra para usar o proxy HTTP e a porta 80.
4.  Clique em “Criar”.

## 13. Listar Regras de Encaminhamento

### Comando `gcloud`

```bash
gcloud compute forwarding-rules list

```

### Pelo Console

1.  Vá para Network Services > Forwarding Rules.
2.  Verifique a lista de regras de encaminhamento.

----------

Seguindo esses passos, você configurará um balanceador de carga HTTP com um grupo de instâncias gerenciadas no Google Cloud. 🚀

```

