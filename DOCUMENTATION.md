# **Run HTML/CSS Application on EC2 with Ubuntu by using Apache and SCP**

[](https://github.com/Afaq-Devxops/Pny-Batch_3_4/blob/EC2/AWS%20EC2%20lab%202%20(Ubuntu%20%26%20SCP).md#run-htmlcss-application-with-apache-on-ubuntu-ec2)

---

#### **Step 1: Launch an Ubuntu EC2 Instance**

[](https://github.com/Afaq-Devxops/Pny-Batch_3_4/blob/EC2/AWS%20EC2%20lab%202%20(Ubuntu%20%26%20SCP).md#step-1-launch-an-ubuntu-ec2-instance)

1. **Log in to the AWS Management Console.**
2. **Launch an Instance:**
    - Select **Ubuntu Server** as the AMI (e.g., Ubuntu 24.04).
    - Choose an instance type like `t2.micro` (free tier eligible).
    - Configure a **Security Group** to allow:
        - **HTTP (Port 80)** from 0.0.0.0/0.
        - **SSH (Port 22)** from your IP (or 0.0.0.0/0 for testing, but this is insecure).
    - Assign or create a **key pair** for SSH access.
     ![[1-launch.png]]

---

#### **Step 2: Connect to the Ubuntu Instance**

[](https://github.com/Afaq-Devxops/Pny-Batch_3_4/blob/EC2/AWS%20EC2%20lab%202%20(Ubuntu%20%26%20SCP).md#step-2-connect-to-the-ubuntu-instance)

1. **SSH into the instance:**
    
    ```shell
    ssh -i "your-key-file.pem" ubuntu@<INSTANCE_PUBLIC_IP>
    ```
   ![[2-ssh.png]]

   ![[3-ssh.png]]

   ![[4-login.png]]


2. **Update the system:**
    
    ```shell
    sudo apt update
    sudo apt upgrade -y
    ```
	 
	 ![[5-update 1.png]]
	 

---

#### **Step 3: Install Apache Web Server**

[](https://github.com/Afaq-Devxops/Pny-Batch_3_4/blob/EC2/AWS%20EC2%20lab%202%20(Ubuntu%20%26%20SCP).md#step-3-install-apache-web-server)

1. **Install Apache:**
    
    ```shell
    sudo apt install apache2 -y
    ```
     ![[6-apache install.png]]
2. **Start and Enable Apache:**
    
    ```shell
    sudo systemctl start apache2
    sudo systemctl enable apache2
    ```
     ![[7-start-enable.png]]
3. **Verify Apache Installation:**
    
    - Open your browser and visit:
        
        ```
        http://<INSTANCE_PUBLIC_IP>
        ```
        
    - You should see the Apache default "It works!" page.
         ![[8-works.png]]

---

#### **Step 4: Deploy Your HTML/CSS Application**

[](https://github.com/Afaq-Devxops/Pny-Batch_3_4/blob/EC2/AWS%20EC2%20lab%202%20(Ubuntu%20%26%20SCP).md#step-4-deploy-your-htmlcss-application)

1. **Upload HTML/CSS Files to the Instance:**
    
    - Use `scp` to transfer files:

[https://github.com/MuzakkirHossainMinhaz/panda-commerce.git](https://github.com/MuzakkirHossainMinhaz/panda-commerce.git)

```
	```bash
    scp -i "your-key-file.pem" path/to/your/files/* ubuntu@<INSTANCE_PUBLIC_IP>:/tmp
    ```
```

       
       ![[9-scp.png]]


  2. **Move Files to Apache’s Web Directory:**
    
    - The default web directory for Apache on Ubuntu is `/var/www/html/`. Move   your files there:
		
```shell
sudo mv /tmp/* /var/www/html/
```
	 
		 ![[10-mv.png]]
		



  3. **Set Proper Permissions:**
    
    - Ensure Apache has the necessary permissions to serve the files:
        
        ```shell
        sudo chmod -R 755 /var/www/html/
        ```

	     ![[11-chmod.png]]
		

   4. **Test Your Application:**
    
    - Visit your public IP in the browser:
        
        ```
        http://<INSTANCE_PUBLIC_IP>
        ```
        
    - Your HTML/CSS application should now load.

        ![[12-webpage.png]]

---

#### **Step 5: Customize Apache Configuration (Optional)**

[](https://github.com/Afaq-Devxops/Pny-Batch_3_4/blob/EC2/AWS%20EC2%20lab%202%20(Ubuntu%20%26%20SCP).md#step-5-customize-apache-configuration-optional)

1. **Enable Directory Indexing (Optional):**
    
    - If your application contains subdirectories and you want directory browsing, enable indexing:
        
        ```shell
        sudo a2enmod autoindex
        sudo systemctl restart apache2
        ```
         ![[14-enabled.png]]
2. **Custom Virtual Host Configuration (Optional):**
    
    - If you plan to host multiple applications or use a custom domain, create a new virtual host file:
        
        ```shell
        sudo nano /etc/apache2/sites-available/your-app.conf
        ```
        ![[15-nano.png]]
    - Add the following configuration:
        
        ```apache
        <VirtualHost *:80>
            ServerAdmin webmaster@localhost
            DocumentRoot /var/www/html
            ErrorLog ${APACHE_LOG_DIR}/error.log
            CustomLog ${APACHE_LOG_DIR}/access.log combined
        </VirtualHost>
        ```
        ![[16-nano.png]]
    - Enable the new configuration:
        
        ```shell
        sudo a2ensite your-app.conf
        sudo systemctl reload apache2
        ```
        ![[17-ensite.png]]

---

#### **Step 6: Secure and Maintain the Instance**

[](https://github.com/Afaq-Devxops/Pny-Batch_3_4/blob/EC2/AWS%20EC2%20lab%202%20(Ubuntu%20%26%20SCP).md#step-6-secure-and-maintain-the-instance)

1. **Limit SSH Access:**
    
    - Edit the security group to allow SSH only from your IP address.
2. **Apply System Updates Regularly:**
    
    ```shell
    sudo apt update && sudo apt upgrade -y
    ```
    
3. **Monitor Logs for Errors:**
    
    - Access logs: `/var/log/apache2/access.log`
    - Error logs: `/var/log/apache2/error.log`

---

#### **Conclusion**

[](https://github.com/Afaq-Devxops/Pny-Batch_3_4/blob/EC2/AWS%20EC2%20lab%202%20(Ubuntu%20%26%20SCP).md#step-7-clean-up-optional)
By following this guide, you’ve successfully deployed an HTML/CSS application on an EC2 instance using Ubuntu and Apache. You can now access your web application from the browser using your EC2 instance’s public IP.

---

This **README.md** gives clear instructions for setting up the EC2 instance, installing Apache, uploading files using SCP, and testing the deployment. Feel free to adapt it based on the specifics of your project.

---
---
