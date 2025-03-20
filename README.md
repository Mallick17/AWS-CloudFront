# **AWS CloudFront Distribution Configuration Settings**
When creating an **Amazon CloudFront distribution**, we configure several settings to control how content is delivered to users.

<details>
  <summary>Steps to Set Up CloudFront Distribution with S3</summary>

## **Steps to Set Up CloudFront Distribution with S3**
### **Step 1: Create an S3 Bucket**
1. Open the AWS Management Console.
2. Navigate to **Amazon S3**.
3. Click **Create Bucket** and provide a unique name.
4. Select a region (e.g., US-East-1).
5. **Block all public access** (recommended).
6. Click **Create Bucket**.

### **Step 2: Upload Static Content**
1. Open the created **S3 bucket**.
2. Click **Upload** and add static files (e.g., images, HTML, CSS).
3. Click **Upload** to store the files.

### **Step 3: Create a CloudFront Distribution**
1. Open the **AWS CloudFront Console**.
2. Click **Create Distribution**.
3. Under **Origin Settings**:
   - **Origin Domain**: Select your S3 bucket.
   - **Origin Access**: Choose **Origin Access Control (OAC)** (recommended for security).
4. Click **Create New Origin Access Control Setting**:
   - Name: Give a meaningful name.
   - Signing Behavior: Select **Sign Requests (Recommended)**.
   - Click **Create**.
5. Enable **Origin Shield** (optional).

### **Step 4: Configure Cache Behavior**
1. **Redirect HTTP to HTTPS**.
2. **Allowed HTTP Methods**:
   - If serving only static files: **GET, HEAD**.
   - If accepting write requests: **GET, HEAD, PUT, POST, PATCH, DELETE**.
3. **Cache Policy**: Choose **Optimized for S3**.

### **Step 5: Enable Web Application Firewall (WAF)**
1. Under **Web Application Firewall Settings**, enable **AWS WAF**.
2. This protects against unwanted traffic and bot attacks.

### **Step 6: Use Edge Locations for Performance**
1. Choose **Use All Edge Locations** for global availability.
2. This ensures content is accessible from the nearest edge location.

### **Step 7: Generate & Apply S3 Bucket Policy**
1. Once CloudFront is created, copy the **CloudFront-generated S3 policy**.
2. Go to **S3 Bucket** > **Permissions** > **Bucket Policy**.
3. Replace any existing policy and **paste the copied policy**.
4. Click **Save Changes**.

### **Step 8: Test CloudFront Distribution**
1. Navigate to **CloudFront Console** > **Distributions**.
2. Copy the **CloudFront Domain Name**.
3. Access files via:
   - `https://<CloudFront_Domain>/object.png`
4. Test HTTP redirection by removing **https** from the URL.
5. The request should redirect to **HTTPS**.

</details>

### **1. Origin Configuration**
The origin is the source from which CloudFront fetches content.

- **Origin Domain**:  
  - This is the source of content (e.g., an **S3 bucket**, an **HTTP server**, or an **AWS VPC origin**).
  - You can select an AWS service or enter a custom DNS name.
  - **Required field**.

- **Origin Path (Optional)**:  
  - A specific path within the origin domain (e.g., `/static-content/`).
  - This is appended to the origin domain for requests.

- **Origin Name**:  
  - A custom name to identify this origin.  
  - **Required field**.

- **Custom Headers (Optional)**:  
  - You can add **custom HTTP headers** that CloudFront includes in requests to the origin.

- **Enable Origin Shield (Yes/No)**:  
  - **Origin Shield** adds an extra caching layer to reduce load on the origin.
  - Helps in **reducing origin traffic** and **improving availability**.

---

### **2. Default Cache Behavior Settings**
These settings control how CloudFront caches content and handles requests.

- **Path Pattern**:  
  - Defines which requests match this behavior.  
  - The default pattern is `"*"` (matches all requests).

- **Compress Objects Automatically (Yes/No)**:  
  - Enables automatic **GZIP/Brotli compression** for supported file types.

- **Viewer Protocol Policy**:  
  - Defines how viewers access content:
    - **HTTP and HTTPS** – Allows both.
    - **Redirect HTTP to HTTPS** – Enforces secure connections.
    - **HTTPS only** – Only HTTPS requests are allowed.

- **Allowed HTTP Methods**:  
  - Defines which HTTP methods are allowed:
    - `GET, HEAD`
    - `GET, HEAD, OPTIONS`
    - `GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE` (for API endpoints).

- **Restrict Viewer Access (Yes/No)**:  
  - If enabled, **signed URLs or signed cookies** are required to access content.

---

### **3. Cache Key and Origin Requests**
Defines how CloudFront caches content and forwards requests.

- **Cache Policy and Origin Request Policy (Recommended)**:  
  - **Cache Policy**: Controls cache key settings (e.g., query strings, headers, and cookies).  
  - **Origin Request Policy**: Defines which headers and cookies are forwarded to the origin.

- **Response Headers Policy (Optional)**:  
  - Configures security and custom headers for responses.

---

### **4. Function Associations (Optional)**
You can associate **CloudFront Functions** or **Lambda@Edge** functions at different request phases.

- **Viewer Request**
- **Viewer Response**
- **Origin Request**
- **Origin Response**  

Each function can modify requests/responses before reaching the origin or the user.

---

### **5. Web Application Firewall (WAF) (Optional)**
- **Enable Security Protections**:  
  - Protects against **DDoS attacks, SQL injection, and XSS**.
  - Uses AWS WAF to block malicious requests.

---

### **6. Price Class**
Determines which **AWS Edge Locations** serve the content:

- **Use all edge locations (best performance)**  
- **Use only North America and Europe**  
- **Use North America, Europe, Asia, Middle East, and Africa**  

Choosing fewer locations **reduces costs** but might **increase latency**.

---

### **7. Alternate Domain Name (CNAME) (Optional)**
- Allows using a **custom domain** instead of the default CloudFront domain.
- Example: `cdn.example.com` instead of `d123.cloudfront.net`.

---

### **8. Custom SSL Certificate (Optional)**
- Uses **AWS Certificate Manager (ACM)** to enable HTTPS with a custom domain.
- Must be in the **us-east-1 (N. Virginia) region**.

---

### **9. Supported HTTP Versions**
Allows additional HTTP versions for improved performance.

- **HTTP/2** (Faster than HTTP/1.1)
- **HTTP/3** (Uses QUIC for low-latency connections)

---

### **10. Default Root Object (Optional)**
- Defines the **default file** to load when a user visits the root URL (`/`).
- Example: `index.html`.

---

### **11. IPv6 Support (On/Off)**
- Enables IPv6 support for modern networks.

---

### **12. Logging (Optional)**
- **Standard Logging (On/Off)**:  
  - Stores request logs in **Amazon S3, CloudWatch, or Kinesis Firehose**.  
  - Logs include **request IPs, response times, and cache hits**.

---

### **Final Step: Create Distribution**
Once all configurations are set, click **Create Distribution** to deploy your CloudFront distribution.

Would you like a step-by-step guide for setting this up in AWS Console or Terraform? 😊
