# 🙋‍♂️ My Networking Module Journey

This README summarizes my hands-on experience with the CoderCo DevOps Academy Networking Module and highlights the key skills I’ve acquired.

---

## 🎓 Course Overview

Over the past week, I  revised my Networking knowledge and dove deep into the fundamentals of how the Internet works:

- **OSI & TCP/IP Models** — Learned each layer’s purpose and real-world protocol mappings.  
- **IP Addressing & Subnetting** — Practiced CIDR notation, calculated subnets, and applied NAT.  
- **DNS Internals** — Explored nameservers, zone files, record types, and name resolution workflows.  
- **Routing Fundamentals** — Compared static vs. dynamic routing and common routing protocols.  
- **Network Troubleshooting** — Used tools like `ping`, `traceroute`, `nslookup`, and `dig`.  
- **Hands-On Deployment** — Deployed an NGINX web server on AWS EC2 and mapped it to a custom domain.

---

## 🛠️ What I Built

1. **EC2 + NGINX**  
   - Launched an Amazon Linux 2023 instance (t3.micro).  
   - Installed and configured NGINX to serve a custom landing page on port 80.  
2. **Custom Domain Mapping**  
   - Purchased `yassinnginx.uk` via Cloudflare.  
   - Created an A-record pointing to my EC2 public IP (DNS Only).  
3. **Configuration Management**  
   - Wrote a dedicated server block in `/etc/nginx/conf.d/yassinnginx.uk.conf`.  
   - Verified syntax with `nginx -t` and applied changes with `systemctl reload nginx`.

---

## 💡 Troubleshooting Highlights

- **Default NGINX Page**  
  Resolved by adding a proper `server_name` directive in my server block.  
- **Stale DNS Entries**  
  Disabled Cloudflare’s proxy (orange cloud) to force direct A-record lookups.  
- **Configuration Reloads**  
  Learned to use `systemctl reload nginx` for zero-downtime updates.

---

## 🏆 Skills Acquired

- **Networking**: IP addressing, subnetting, DNS architecture, routing basics  
- **Linux**: SSH access, package management (`yum`), file editing (`vi`)  
- **Web Servers**: NGINX installation, virtual hosts, configuration testing  
- **Cloud**: EC2 instance provisioning, security groups, elastic IPs  
- **Debugging**: CLI tools for network diagnostics and log inspection  
- **Version Control**: Documenting projects in GitHub with clear README and folder structure

---

## 🚀 What’s Next?

I’m excited to apply these networking fundamentals to more complex cloud architectures and begin automating deployments using advanced DevOps tools.  

> _“Networking is the backbone of every cloud architecture—master it, and the sky is the limit!”_  
