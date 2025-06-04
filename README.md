# Wiki-Setting-up-R-4.4.1-and-RStudio-Server-on-Ubuntu-Server-22.04-with-CRAN-Repository
## Background
This guide covers the steps to install R 4.4.1 and configure RStudio Server on Ubuntu 22.04 using the official CRAN repository. Network restrictions initially prevented direct CRAN access, so manual key installation and repository setup were necessary. It also covers compatibility notes between R versions and RStudio Server on this system.

---

## Step 1: Configure CRAN Repository Access on Ubuntu 22.04

### Problem: SSH Bastion Restrictions
- The SSH bastion blocked direct access to CRAN repositories.

### Solution:
- Download and add the CRAN public key:

```bash
curl -fsSL https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | sudo gpg --dearmor -o /usr/share/keyrings/cran-archive-keyring.gpg
```

- Add the CRAN repository to your sources:

```bash
echo "deb [signed-by=/usr/share/keyrings/cran-archive-keyring.gpg] https://cloud.r-project.org/bin/linux/ubuntu jammy-cran40/" | sudo tee /etc/apt/sources.list.d/cran.list
```

- Update package lists:

```bash
sudo apt update
```

---

## Step 2: Install R 4.4.1 on Ubuntu 22.04

- List available R versions:

```bash
apt-cache policy r-base
```

- Install R 4.4.1 explicitly:

```bash
sudo apt install r-base-core=4.4.1-3.2204.0
```

- Verify R version:

```bash
R --version
```

---

## Step 3: Remove Older R Versions

- Remove old R 4.1.2 installation to avoid conflicts:

```bash
sudo apt remove r-base-core=4.1.2-1ubuntu2
sudo apt autoremove
```

- Confirm removal:

```bash
dpkg -l | grep r-base
```

---

## Step 4: Install and Start RStudio Server

- Install RStudio Server (replace `<version>` with your downloaded package version):

```bash
sudo dpkg -i rstudio-server-<version>.deb
```

- Start and enable the service:

```bash
sudo systemctl start rstudio-server
sudo systemctl enable rstudio-server
```

- Check status:

```bash
sudo systemctl status rstudio-server
```

---

## Step 5: Access RStudio Server as a User

- Access via browser:

```
http://<server-ip>:8787
```

- Or create SSH tunnel (replace hostnames as needed):

```bash
ssh -L 58787:c8pu01:8787 grathore@facbastp0008.campus.net.ucsf.edu
```

- Then open:

```
http://localhost:58787
```

- Login with system user credentials.

---

## Compatibility Report: R 4.5 vs RStudio Server on Ubuntu 22.04

- R 4.5.0 is **not stable or compatible** with current RStudio Server versions on Ubuntu 22.04.
- R 4.4.1 is **stable and works well** with RStudio Server.
- Recommendation: Use R 4.4.1 until official support for R 4.5 is available.

---

## Summary of Commands

| Step                                | Command/Action                                         |
|-----------------------------------|-------------------------------------------------------|
| Add CRAN key                      | `curl -fsSL https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc \| sudo gpg --dearmor -o /usr/share/keyrings/cran-archive-keyring.gpg` |
| Add CRAN repo                    | `echo "deb [signed-by=/usr/share/keyrings/cran-archive-keyring.gpg] https://cloud.r-project.org/bin/linux/ubuntu jammy-cran40/" \| sudo tee /etc/apt/sources.list.d/cran.list` |
| Update package lists             | `sudo apt update`                                      |
| Remove old R 4.1                 | `sudo apt remove r-base-core=4.1.2-1ubuntu2`          |
| Install R 4.4.1 core             | `sudo apt install r-base-core=4.4.1-3.2204.0`          |
| Install RStudio Server           | `sudo dpkg -i rstudio-server-<version>.deb`            |
| Start RStudio Server             | `sudo systemctl start rstudio-server`                   |
| Enable auto-start RStudio Server | `sudo systemctl enable rstudio-server`                  |
| Access RStudio Server            | `http://<server-ip>:8787` or SSH tunnel                 |

---

# Notes
- Replace placeholders like `<version>` and `<server-ip>` with actual values.
- Make sure your network allows required ports or use SSH tunneling as shown.
- Always verify RStudio Server compatibility with your R version before upgrading.

---

Feel free to customize or extend this README as needed.
