## ⚡ Launching an NGINX Web Server on Amazon EC2 + Custom Domain (Cloudflare) 🌐

I built my own web server on AWS EC2 and mapped it to a custom domain via Cloudflare.  
In this guide, I’ll walk you through the exact steps I took — including the struggles, configs, and final success.

---

## 🔹 Step 1: Purchase a Domain via Cloudflare

Go to https://dash.cloudflare.com and buy a domain.  
It should cost around **$5–$10/year** depending on the extension (`.uk`, `.dev`, `.me`, etc).

---

## 🔹 Step 2: Launch an EC2 Instance on AWS

- AMI: **Amazon Linux 2023**
- Instance Type: **t3.micro (Free tier eligible)**
- Security Group Inbound Rules:
  - **SSH (22)** → Your IP only
  - **HTTP (80)** → 0.0.0.0/0
  - **HTTPS (443)** → 0.0.0.0/0

---

## 🔹 Step 3: Get the Public IPv4 Address

After launching, go to the EC2 dashboard → Instances  
Find your instance and note the **Public IPv4 address**.

---

## 🔹 Step 4: SSH Into the EC2 Instance

```bash
ssh -i /path/to/your-key.pem ec2-user@<EC2_PUBLIC_IP>
```

---

## 🔹 Step 5: Install & Start NGINX

```bash
sudo yum update -y
sudo yum install -y nginx
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

## 🔹 Step 6: Point Domain to EC2 via A Record

In Cloudflare DNS settings:
- **Type**: A  
- **Name**: `@` or `www`  
- **IPv4 address**: `<your-ec2-public-ip>`  
- **Proxy status**: DNS Only (disable orange cloud)

---

## 🔹 Step 7: Confirm DNS Propagation

```bash
nslookup yassinnginx.uk
```

Expected output:

```
Non-authoritative answer:
Name:   yourdomain.com
Address: <EC2_PUBLIC_IP>
```

---

## 🔹 Step 8: Configure NGINX for Your Domain

```bash
sudo vi /etc/nginx/conf.d/yassinnginx.uk.conf
```

Paste:

```nginx
server {
    listen 80;
    server_name http://yassinnginx.uk

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Then:

```bash
sudo nginx -t
sudo systemctl reload nginx
```

---

## 🔹 Step 9: Customize the NGINX Default Page

```bash
sudo vi /usr/share/nginx/html/index.html
```

Paste:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Yassin Suleiman | DevOps Engineer</title>
  <style>
    body { background:#0f172a; color:#f1f5f9; font-family:Arial; text-align:center; padding-top:20vh; }
    h1 { font-size:3rem; margin-bottom:1rem; }
    h2 { font-size:1.5rem; color:#38bdf8; margin-bottom:2rem; }
    a { color:#facc15; text-decoration:none; margin:0 1rem; font-size:1.2rem; }
  </style>
</head>
<body>
  <h1>Yassin Suleiman</h1>
  <h2>DevOps Engineer | Cloud | Linux | CI/CD</h2>
  <div>
    <a href="https://github.com/yassinsuleiman">GitHub</a>
    <a href="mailto:yassine.suleiman@proton.me">Email</a>
    <a href="https://linkedin.com/in/ysuleiman">LinkedIn</a>
  </div>
</body>
</html>
```

Then:

```bash
sudo systemctl reload nginx
```

---

## 🔹 Step 10: Final Test

Visit:

```
http://yassinnginx.uk
```

You should see your personal NGINX landing page.

---

## 🧠 Troubleshooting + Lessons Learned
During setup I hit several roadblocks—here’s what went wrong and how I fixed each one:

**❌ Issue: Still seeing default NGINX landing page**
- **Symptom:** Browsing `http://yassinnginx.uk` showed the generic "Welcome to nginx!" page.
- **Root Cause:** No server block was defined for my domain, so NGINX served its default site.
- **Fix:** Created `/etc/nginx/conf.d/yassinnginx.uk.conf` with a `server_name yassinnginx.uk www.yassinnginx.uk;` directive, pointing `root` to `/usr/share/nginx/html`.

**✅ Validation:** Ran `sudo nginx -t` → configuration OK.

**❌ Issue: Configuration not applied**
- **Symptom:** Updates to the server block didn’t appear in the browser.
- **Root Cause:** NGINX wasn’t reloading its config after edits.
- **Fix:** Executed `sudo systemctl reload nginx` (instead of restart) to apply changes without downtime.

**✅ Validation:** `curl -I http://yassinnginx.uk` returned `HTTP/1.1 200 OK` with my custom page headers.

**⚠️ DNS Tip: Cloudflare Proxy vs. DNS Only**
- **Symptom:** Domain still pointed to an old A record or showed error 521.
- **Root Cause:** Cloudflare’s "Orange Cloud" proxy was caching or blocking direct IP resolution.
- **Fix:** Switched the A record to **DNS only** (gray cloud), ensuring direct lookups to my EC2 IP.

**🧠 Key Takeaways:
- Ensure only one active server block per domain; I had to **delete the default `/etc/nginx/nginx.conf` server block** to avoid conflicts.
- Always match `server_name` exactly to your domain.
- Verify syntax with `nginx -t` before reloading.
- Use `systemctl reload` to apply config edits.
- For initial testing, disable Cloudflare proxy so DNS resolves directly.

---

## ✅ Recap

- [x] Bought domain
- [x] Launched EC2
- [x] Installed NGINX
- [x] Created A record in Cloudflare
- [x] Built NGINX server block
- [x] Customized page
- [x] DNS propagation confirmed

---

## 🎉 End Result

You now have a working NGINX server running on EC2, mapped to a real domain.  
Perfect to use as your portfolio, a landing page, or further deploy CI/CD apps on top.
