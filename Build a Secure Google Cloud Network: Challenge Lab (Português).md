# Build a Secure Google Cloud Network

## Task 1: Iniciar a Instância Bastion
1. No **Menu de Navegação**, acesse **Compute Engine** > **VM instances**.
2. Temos duas instâncias: `bastion` (parada) e `juice-shop` (rodando). Clique no ícone de três pontos ao lado da instância `bastion` e selecione **Start/Resume**.

## Task 2: Configurar Regras de Firewall
1. Execute os seguintes comandos no terminal Cloud Shell:
    ```sh
    export IAP_NETWORK_TAG=[substitua pelo seu SSH IAP network tag]
    export INTERNAL_NETWORK_TAG=[substitua pelo seu SSH internal network tag]
    export HTTP_NETWORK_TAG=[substitua pelo seu HTTP network tag]
    export ZONE=[substitua pela sua Zone]

    gcloud compute firewall-rules delete open-access

    gcloud compute firewall-rules create ssh-ingress --allow=tcp:22 --source-ranges 35.235.240.0/20 --target-tags $IAP_NETWORK_TAG --network acme-vpc

    gcloud compute instances add-tags bastion --tags=$IAP_NETWORK_TAG --zone=$ZONE

    gcloud compute firewall-rules create http-ingress --allow=tcp:80 --source-ranges 0.0.0.0/0 --target-tags $HTTP_NETWORK_TAG --network acme-vpc

    gcloud compute instances add-tags juice-shop --tags=$HTTP_NETWORK_TAG --zone=$ZONE

    gcloud compute firewall-rules create internal-ssh-ingress --allow=tcp:22 --source-ranges 192.168.10.0/24 --target-tags $INTERNAL_NETWORK_TAG --network acme-vpc

    gcloud compute instances add-tags juice-shop --tags=$INTERNAL_NETWORK_TAG --zone=$ZONE
    ```

2. Digite **Y** se solicitado e aperte **ENTER**.

## Task 3: Conectar via SSH na Instância Juice-Shop
1. Após o processo ser concluído, clique no botão **SSH** da instância `bastion` e execute o seguinte comando:
    ```sh
    gcloud compute ssh juice-shop --internal-ip
    ```

2. Digite **Y** se solicitado e aperte **ENTER**.
3. Aperte a tecla **ENTER** duas vezes para confirmar a senha vazia e, por fim, digite **Y**.

**Congratulations!**
