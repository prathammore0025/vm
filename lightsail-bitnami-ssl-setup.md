# 🔒 Enabling HTTPS/SSL on Bitnami WordPress (AWS Lightsail)

This guide walks you through configuring **Let's Encrypt SSL** for a **Bitnami WordPress stack** on AWS Lightsail using the built-in **`bncert-tool`**.

---

## 📌 Prerequisites

- A running **Lightsail** instance with **Bitnami WordPress**.
- A registered domain/subdomain pointing to the instance's **public IP address** via **DNS A record**.

> 🔗 Example:  
> `devops.prathm.com` → A Record → `34.229.X.X` (your Lightsail instance IP)

To test if DNS is working:  
**Check:** [http://test.verticalclick.us/](http://test.verticalclick.us/)  
If it loads the WordPress page, proceed.

---

## 🔐 Steps to Enable HTTPS with Let's Encrypt

### ✅ 1. Connect to the Lightsail instance

Go to your AWS Lightsail dashboard and use the “**Connect using SSH**” option.  
Alternatively, use terminal:

```bash
ssh -i /path/to/key.pem bitnami@<your-lightsail-ip>
```

---

### ⚙️ 2. Run the Bitnami HTTPS Configuration Tool

```bash
sudo /opt/bitnami/bncert-tool
```

This tool:
- Requests and installs an SSL certificate from Let’s Encrypt.
- Updates Apache configs.
- Sets up automatic renewal.

---

### 🧭 3. Follow the Interactive Prompts

```bash
Domain list []: test.verticalclick.us
```

> ⚠️ If you're using a **subdomain only** (like `test.verticalclick.us`), choose `n` when asked to include `www.`

```bash
The following domains were not included: www.test.verticalclick.us. Do you want to add them? [Y/n]: n
```

Enable HTTPS redirection:

```bash
Enable HTTP to HTTPS redirection [Y/n]: Y
```

Confirm all changes:

```bash
Do you agree to these changes? [Y/n]: Y
```

Enter your email for renewal notifications:

```bash
E-mail address []: prathamesh@underpinservices.com
```

Agree to Let's Encrypt Terms:

```bash
Do you agree to the Let's Encrypt Subscriber Agreement? [Y/n]: Y
```

---

### 📈 4. Success Output Example

```text
----------------------------------------------------------------------------
Success

The Bitnami HTTPS Configuration Tool succeeded in modifying your installation.

The configuration report is shown below.

Backup files:
* /opt/bitnami/apache/conf/httpd.conf.back.202505091445
* /opt/bitnami/apache/conf/bitnami/bitnami.conf.back.202505091445
...

Find more details in the log file:
/tmp/bncert-202505091445.log

Press [Enter] to continue:
```

This confirms the SSL was successfully configured.

---

## 🔁 5. Auto-Renewal Configuration

The `bncert-tool` sets up a `cron` job for automatic certificate renewal. You can verify it:

```bash
crontab -l
```

Expected entry:

```bash
0 0 * * * /opt/bitnami/letsencrypt/lego --tls --email="prathamesh@underpinservices.com" --domains="test.verticalclick.us" --path="/opt/bitnami/letsencrypt" renew && /opt/bitnami/ctlscript.sh restart apache
```

---

## 🌐 6. Verify SSL

Now visit:  
➡️ [https://test.verticalclick.us](https://test.verticalclick.us)  
You should see a **padlock icon 🔒** and the **HTTPS** URL.

---

## 🔧 7. Firewall & Port Configuration (If Needed)

Make sure port **443** (HTTPS) is allowed:

1. Go to **Lightsail > Networking > Instance > Firewall**.
2. Add rule:
   - **Application**: HTTPS
   - **Port**: 443
   - **Protocol**: TCP

---

## ✅ Summary

| Step | Task |
|------|------|
| ✅ 1 | Connect to instance |
| ✅ 2 | Run `bncert-tool` |
| ✅ 3 | Enter domain and options |
| ✅ 4 | Success message confirms setup |
| ✅ 5 | Certificate auto-renewal |
| ✅ 6 | Verify via browser |
| ✅ 7 | Open HTTPS port in Lightsail |

---

**Generated on:** 2025-05-09 15:03:07
