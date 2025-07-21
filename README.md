# **Practical Session: Initialize a React Project & Push to GitHub**  

### **Objective:**  
By the end of this session, you will be able to:  
✅ Set up a React project locally.  
✅ Push your code to a GitHub repository.  

---

## **🔧 Prerequisites**  
Before starting, ensure you have:  

### **1. Install Node.js** (Required to run React)  
- **Check if Node.js is already installed:**  
  Open **Command Prompt (Windows) / Terminal (Mac/Linux)** and run:  
  ```sh
  node -v
  ```  
  - If you see a version (e.g., `v18.12.1`), you’re good to go!  
  - If not, download and install the **LTS (Long-Term Support) version** from:  
    🔗 [https://nodejs.org](https://nodejs.org)  

### **2. Install Git** (Required to upload code to GitHub)  
- **Check if Git is installed:**  
  Run in Command Prompt/Terminal:  
  ```sh
  git --version
  ```  
  - If you see a version (e.g., `git version 2.39.1`), Git is installed.  
  - If not, download Git from:  
    🔗 [https://git-scm.com/downloads](https://git-scm.com/downloads)  

---

## **🚀 Step-by-Step Guide**  

### **Step 1: Create a GitHub Repository**  
1. Go to [GitHub.com](https://github.com) and log in.  
2. Click **"New"** (Green button) to create a repository.  
3. Enter a name (e.g., `react-project-your-student-id`).  
4. Keep it **Public** (for now).  
5. Click **"Create repository."**  

### **Step 2: Clone the Repository to Your Computer**  
1. Open **Command Prompt/Terminal** and navigate to your desired folder (e.g., `Desktop`).  
2. Run:  
   ```sh
   git clone https://github.com/your-username/repository-name.git
   ```  
   (Replace the URL with your actual GitHub repo link.)  

### **Step 3: Create a React App Inside the Cloned Folder**  
1. Navigate into your repo folder:  
   ```sh
   cd repository-name
   ```  
2. Create a React app:  
   ```sh
   npx create-react-app my-react-app-name-here
   ```  
   (Replace `my-react-app-name-here` with your preferred name.)  
3. Wait for installation (may take 1-2 minutes).  

### **Step 4: Push the Code to GitHub**  
1. Go back to the **root folder** of your project:  
   ```sh
   cd ..
   ```  
2. Stage, commit, and push your code:  
   ```sh
   git add .
   git commit -m "Initial React project setup"
   git push origin main
   ```  
3. Refresh your GitHub repo page—your code should now appear!  

---

## **💡 Troubleshooting Tips**  
❌ **"Command not found" errors?**  
→ Ensure Node.js/Git is installed correctly. Restart your terminal.  

❌ **GitHub permission issues?**  
→ Make sure you cloned the correct repo URL and are logged into GitHub.  

❌ **React app not working?**  
→ Run `npm start` inside your React project folder to test it locally first.  

---

