# Build a Secure Google Cloud Network

## Task 1: Start the Bastion Instance
1. In the **Navigation Menu**, go to **Compute Engine** > **VM instances**.
2. We have two instances: `bastion` (stopped) and `juice-shop` (running). Click on the three-dot icon next to the `bastion` instance and select **Start/Resume**.

## Task 2: Configure Firewall Rules
1. Execute the following commands in the Cloud Shell terminal:
    ```sh
    export IAP_NETWORK_TAG=[replace with your SSH IAP network tag]
    export INTERNAL_NETWORK_TAG=[replace with your SSH internal network tag]
    export HTTP_NETWORK_TAG=[replace with your HTTP network tag]
    export ZONE=[replace with your Zone]

    gcloud compute firewall-rules delete open-access

    gcloud compute firewall-rules create ssh-ingress --allow=tcp:22 --source-ranges 35.235.240.0/20 --target-tags $IAP_NETWORK_TAG --network acme-vpc

    gcloud compute instances add-tags bastion --tags=$IAP_NETWORK_TAG --zone=$ZONE

    gcloud compute firewall-rules create http-ingress --allow=tcp:80 --source-ranges 0.0.0.0/0 --target-tags $HTTP_NETWORK_TAG --network acme-vpc

    gcloud compute instances add-tags juice-shop --tags=$HTTP_NETWORK_TAG --zone=$ZONE

    gcloud compute firewall-rules create internal-ssh-ingress --allow=tcp:22 --source-ranges 192.168.10.0/24 --target-tags $INTERNAL_NETWORK_TAG --network acme-vpc

    gcloud compute instances add-tags juice-shop --tags=$INTERNAL_NETWORK_TAG --zone=$ZONE
    ```

2. Type **Y** if prompted and press **ENTER**.

## Task 3: SSH into the Juice-Shop Instance
1. After the process is completed, click the **SSH** button for the `bastion` instance and execute the following command:
    ```sh
    gcloud compute ssh juice-shop --internal-ip
    ```

2. Type **Y** if prompted and press **ENTER**.
3. Press **ENTER** twice to confirm the empty password and finally type **Y**.

**Congratulations!**
