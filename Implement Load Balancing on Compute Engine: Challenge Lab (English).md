### English
# Implement Load Balancing on Compute Engine: Challenge Lab

This step-by-step guide will help you set up load balancing on Google Cloud using `gcloud` commands and console alternatives.

## Environment Variables

Set the environment variables to simplify command execution:

```bash
export INSTANCE=<instance-name>
export FIREWALL=<firewall-rule-name>
export ZONE=<your-zone>
export REGION=<your-region>
```

## 1. Create VM Instance

### `gcloud` Command

```bash
gcloud compute instances create $INSTANCE \
    --zone=$ZONE \
    --machine-type=e2-micro
```

### Using the Console

1. Go to Compute Engine > VM Instances.
2. Click "Create Instance".
3. Configure the instance as needed and click "Create".

## 2. Startup Script

Create a startup script to install and configure Nginx:

```bash
cat << EOF > startup.sh
#! /bin/bash
apt-get update
apt-get install -y nginx
service nginx start
sed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.html
EOF
```

## 3. Create Instance Template

### `gcloud` Command

```bash
gcloud compute instance-templates create web-server-template \
    --metadata-from-file startup-script=startup.sh \
    --machine-type g1-small \
    --region $REGION
```

### Using the Console

1. Go to Compute Engine > Instance Templates.
2. Click "Create Instance Template".
3. Add the startup script in the "Startup script" section.
4. Configure the rest as needed and click "Create".

## 4. Create Managed Instance Group

### `gcloud` Command

```bash
gcloud compute instance-groups managed create web-server-group \
    --base-instance-name web-server \
    --size 2 \
    --template web-server-template \
    --region $REGION
```

### Using the Console

1. Go to Compute Engine > Instance Groups.
2. Click "Create Instance Group".
3. Select the instance template created earlier.
4. Configure the rest as needed and click "Create".

## 5. Create Firewall Rule

### `gcloud` Command

```bash
gcloud compute firewall-rules create $FIREWALL \
    --allow tcp:80 \
    --network default
```

### Using the Console

1. Go to VPC Network > Firewall Rules.
2. Click "Create Firewall Rule".
3. Configure the rule to allow TCP traffic on port 80.
4. Click "Create".

## 6. Create HTTP Health Check

### `gcloud` Command

```bash
gcloud compute http-health-checks create http-basic-check
```

### Using the Console

1. Go to Network Services > Health Checks.
2. Click "Create Health Check".
3. Configure the HTTP health check and click "Create".

## 7. Configure Named Ports

### `gcloud` Command

```bash
gcloud compute instance-groups managed \
    set-named-ports web-server-group \
    --named-ports http:80 \
    --region $REGION
```

### Using the Console

1. Go to Compute Engine > Instance Groups.
2. Select the managed instance group.
3. Click "Edit" and add the named port `http:80`.
4. Click "Save".

## 8. Create Backend Service

### `gcloud` Command

```bash
gcloud compute backend-services create web-server-backend \
    --protocol HTTP \
    --http-health-checks http-basic-check \
    --global
```

### Using the Console

1. Go to Network Services > Backend Services.
2. Click "Create Backend Service".
3. Configure the backend service to use the managed instance group and the health check.
4. Click "Create".

## 9. Add Backend to Backend Service

### `gcloud` Command

```bash
gcloud compute backend-services add-backend web-server-backend \
    --instance-group web-server-group \
    --instance-group-region $REGION \
    --global
```

### Using the Console

1. Go to Network Services > Backend Services.
2. Select the created backend service.
3. Add the managed instance group as a backend.
4. Click "Save".

## 10. Create URL Map

### `gcloud` Command

```bash
gcloud compute url-maps create web-server-map \
    --default-service web-server-backend
```

### Using the Console

1. Go to Network Services > URL Maps.
2. Click "Create URL Map".
3. Configure the URL map to use the backend service.
4. Click "Create".

## 11. Create HTTP Proxy

### `gcloud` Command

```bash
gcloud compute target-http-proxies create http-lb-proxy \
    --url-map web-server-map
```

### Using the Console

1. Go to Network Services > Target Proxies.
2. Click "Create HTTP Proxy".
3. Configure the proxy to use the URL map.
4. Click "Create".

## 12. Create Forwarding Rule

### `gcloud` Command

```bash
gcloud compute forwarding-rules create http-content-rule \
    --global \
    --target-http-proxy http-lb-proxy \
    --ports 80
```

### Using the Console

1. Go to Network Services > Forwarding Rules.
2. Click "Create Forwarding Rule".
3. Configure the rule to use the HTTP proxy and port 80.
4. Click "Create".

## 13. List Forwarding Rules

### `gcloud` Command

```bash
gcloud compute forwarding-rules list
```

### Using the Console

1. Go to Network Services > Forwarding Rules.
2. Verify the list of forwarding rules.

---

By following these steps, you will set up an HTTP load balancer with a managed instance group on Google Cloud. 🚀
```

