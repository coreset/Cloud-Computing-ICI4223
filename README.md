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
542762bef7e4
```

---

## **🚀 Step-by-Step Guide**

### **Step 1: Create a DigitalOcean Account**
1. Go to [https://www.digitalocean.com](https://www.digitalocean.com)
2. Click **"Sign Up"**
3. Fill in your **email and password** or sign up with GitHub/Google.
4. Navigate to the billing page by proceeding through the Droplet creation process:

   ```
   d1c4c6e66979
   ```
   ![Create Droplet](https://github.com/coreset/Cloud-Computing-ICI4223/blob/Practical-Session-02-setup-digital-ocean-vps/images/image-0.png)

   ![Go to Add Payment Method](https://github.com/coreset/Cloud-Computing-ICI4223/blob/Practical-Session-02-setup-digital-ocean-vps/images/image-1.png)

5. Add Promo Code to under Promos section.
   ![Add Promos](https://github.com/coreset/Cloud-Computing-ICI4223/blob/Practical-Session-02-setup-digital-ocean-vps/images/image-2.png)
6. Add your payment method (card or PayPal).
   ![Add bank card](https://github.com/coreset/Cloud-Computing-ICI4223/blob/Practical-Session-02-setup-digital-ocean-vps/images/image-4.png)
7. Check success response for PromoCode.
   ![Get success toast message](https://github.com/coreset/Cloud-Computing-ICI4223/blob/Practical-Session-02-setup-digital-ocean-vps/images/image-3.png)
   ![Get your credit offer](https://github.com/coreset/Cloud-Computing-ICI4223/blob/Practical-Session-02-setup-digital-ocean-vps/images/image-5.png)


---

### **Step 2: Create a Droplet (VPS)**
1. After login, go to the **DigitalOcean Dashboard**.
2. Click **"Create" → "Droplets"**

   ![Create Droplet](https://github.com/coreset/Cloud-Computing-ICI4223/blob/Practical-Session-02-setup-digital-ocean-vps/images/image-5.png)

3. Choose the following options:
   - **Data Center Region:** Choose nearest (e.g., Bangalore, Singapore)
    ![Data Center Region Options](https://github.com/coreset/Cloud-Computing-ICI4223/blob/Practical-Session-02-setup-digital-ocean-vps/images/image-7.png)
   - **Image:** Ubuntu 22.04 (LTS)
   - **Plan:** Regular **CPU Options:**  (credits will cover this)
   ![Data Center Region Options](https://github.com/coreset/Cloud-Computing-ICI4223/blob/Practical-Session-02-setup-digital-ocean-vps/images/image-7.png)

   - **Authentication:**
     - Select **"Password"**
     - Set a **strong root password** and remember it
   ![Data Center Region Options](https://github.com/coreset/Cloud-Computing-ICI4223/blob/Practical-Session-02-setup-digital-ocean-vps/images/image-9.png)
  - **Add student id as Hostname**
   ![Change Hostname](https://github.com/coreset/Cloud-Computing-ICI4223/blob/Practical-Session-02-setup-digital-ocean-vps/images/image-10.png)
  
5. Click **"Create Droplet"**
6. Wait until it says **"Your droplet is ready"**

   ![Droplet Ready]((https://github.com/coreset/Cloud-Computing-ICI4223/blob/Practical-Session-02-setup-digital-ocean-vps/images/image-11.png)

---

### **Step 3: Connect to Your Droplet via SSH**
💻 Open **Terminal** (Mac/Linux) or **git bash** (Windows):

```sh
$ ssh root@your_droplet_ip
# If the above SSH command does not work due to VPS SSH configuration issues, try the following alternative command
$ ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no root@your_droplet_ip
```

- Replace `your_droplet_ip` with the actual IP address shown in your droplet dashboard.
  ![Droplet IP](https://github.com/coreset/Cloud-Computing-ICI4223/blob/Practical-Session-02-setup-digital-ocean-vps/images/image-12.png)
- Enter the **root password** you set earlier.
- If asked to confirm connection, type:
  ```sh
  yes
  ```


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
