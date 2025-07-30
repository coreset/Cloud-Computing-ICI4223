# **Practical Session: Create a DigitalOcean VPS (Droplet)**

### **🎯 Objective**
By the end of this session, you will be able to:  
✅ Create a DigitalOcean account using a promo code.  
✅ Launch a Virtual Private Server (droplet).  
✅ Connect to your server using SSH.

---

## **🔧 Prerequisites**
Before starting, make sure you have:

### **1. A Valid Email Address**
Required to sign up for DigitalOcean.

### **2. A Payment Method (Credit/Debit Card)**
DigitalOcean requires this to activate the free credit.

---

## **🎁 Promo Code**
Use this promo code during signup to receive **$200 free credits (valid for 60 days):**

```
d1c4c6e66979
```

---

## **🚀 Step-by-Step Guide**

### **Step 1: Create a DigitalOcean Account**
1. Go to [https://www.digitalocean.com](https://www.digitalocean.com)
2. Click **"Sign Up"**

   ![Sign Up Page](./images/sign-up-page.png)

3. Fill in your **email and password** or sign up with GitHub/Google.
4. On the billing page, enter the promo code:

   ```
   xxxxxx
   ```

   ![Promo Code Entry](./Practical-Session-02-setup-digital-ocean-vps/Screenshot%20from%20Screencast%20From%202025-07-30%2023-36-20.mp4%20-%201.png)

5. Add your payment method (card or PayPal).
6. Click **"Submit"** to complete the signup.

---

### **Step 2: Create a Droplet (VPS)**
1. After login, go to the **DigitalOcean Dashboard**.
2. Click **"Create" → "Droplets"**

   ![Create Droplet](./images/create-droplet.png)

3. Choose the following options:
   - **Image:** Ubuntu 22.04 (LTS)
   - **Plan:** Basic
   - **CPU Options:** Regular (Shared CPU)
   - **Choose a plan:** $4/month is enough (credits will cover this)
   - **Data Center Region:** Choose nearest (e.g., Bangalore, Singapore)
   - **Authentication:**
     - Select **"Password"**
     - Set a **strong root password** and remember it

   ![Droplet Configuration](./images/droplet-config.png)

4. Click **"Create Droplet"**
5. Wait until it says **"Your droplet is ready"**

   ![Droplet Ready](./images/droplet-ready.png)

---

### **Step 3: Connect to Your Droplet via SSH**
💻 Open **Terminal** (Mac/Linux) or **Command Prompt** (Windows with OpenSSH installed):

```sh
ssh root@your_droplet_ip
```

- Replace `your_droplet_ip` with the actual IP address shown in your droplet dashboard.
- Enter the **root password** you set earlier.
- If asked to confirm connection, type:
  ```sh
  yes
  ```

✅ You are now connected to your VPS!

---

## **🔐 Optional: Secure Your Droplet**
For production or future use, consider:
- Adding a new user (not using `root`)
- Setting up SSH key-based authentication
- Installing a firewall:

```sh
ufw allow OpenSSH
ufw enable
```

---

## **💡 Troubleshooting Tips**

❌ **SSH not connecting?**  
→ Make sure your IP address is correct and droplet is active.  
→ Try:
```sh
ping your_droplet_ip
```

❌ **Permission denied error?**  
→ Double-check the root password and IP address.

❌ **Promo code not working?**  
→ Ensure you're using a **new account** and entering the code correctly.  
→ Contact [DigitalOcean Support](https://www.digitalocean.com/support) if needed.

---
