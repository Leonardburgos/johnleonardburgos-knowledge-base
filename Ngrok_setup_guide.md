# Configuring NGROK

## Step 1: Create an NGROK Account
To begin using NGROK, you need to create an account:

1. Visit [NGROK's official website](https://ngrok.com/).
2. Sign up or log in to your account.
3. After logging in, you will be provided with an authentication token. Save this for later use.

## Step 2: Download NGROK
Follow these instructions to download and set up NGROK:

1. Visit the [NGROK download page](https://download.ngrok.com/windows).
2. Download the version of NGROK that corresponds to your operating system.
3. Extract the NGROK executable (`ngrok.exe`) to a location of your choice on your system.
4. Open ngrok.exe

## Step 3: Authenticate with NGROK
Once NGROK is downloaded, open a command prompt or terminal and run the following command to authenticate NGROK with your account:

```bash
ngrok config add-authtoken $YOUR_AUTHTOKEN
```

Replace `$YOUR_AUTHTOKEN` with the token you obtained when you created your NGROK account.

## Step 4: Configure NGROK
To configure NGROK for your specific use case, you need to edit the configuration file. The NGROK configuration file is located at:

```bash
User/AppData/Local/ngrok/ngrok.yml
```

#### Example Configuration:

Edit the `ngrok.yml` file to define tunnels for exposing your services. Here is an example configuration for a React web app and a Flask API:

```yaml
version: "3"
agent:
  authtoken: <YOUR_AUTHTOKEN>  # Replace with your NGROK auth token
tunnels:
  # Tunnel for your React Web App
  webapp:
    addr: http://localhost:3000       # Local port for your React app
    proto: http                       # Use HTTP protocol
    host_header: passthrough         # Pass headers without modification
    inspect: false                   # Disable inspection (prevents header addition)
  
  # Tunnel for your Flask API
  api:
    addr: https://localhost:5000      # Local port for your Flask API
    labels:
      - <your_edge>  # Optional: Replace with a label from your NGROK dashboard
```

## Step 5: Add a Custom Domain (Optional)
If you want to map a custom domain to your NGROK tunnel, you can add a `domain` field to the configuration:

```yaml
domain: <yourdomain>   # Replace with your custom domain
```

## Step 6: Start NGROK
After configuring the `ngrok.yml` file, start NGROK by running the following command in the terminal:

```bash
ngrok start --all
```

This command will start all the tunnels specified in your configuration file.

## Step 7: Access Your Tunnels
Once NGROK is running, it will provide you with URLs that map to your local services. For example:

- For your React web app: `http://<subdomain>.ngrok.io`
- For your Flask API: `https://<subdomain>.ngrok.io`

These URLs are now publicly accessible and can be shared or used in development.

## Step 8: Additional Configuration (Optional)
You can add more configurations to NGROK, such as:

- **Inspecting traffic**: Set `inspect: true` to enable traffic inspection.
- **TCP/UDP Tunnels**: If you need to tunnel other protocols, refer to the [NGROK documentation](https://ngrok.com/docs) for more details on additional configuration options.
