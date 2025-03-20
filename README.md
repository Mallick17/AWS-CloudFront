# CloudFront (Securely deliver content with low latency and high transfer speeds)
- Amazon CloudFront is a fast content delivery network (CDN) service that securely delivers data, videos, applications, and APIs to customers globally with low latency and high transfer speeds.
- Amazon CloudFront is a web service that speeds up distribution of your static and dynamic web content, such as .html, .css, .js, and image files, to your users. CloudFront delivers your content through a worldwide network of data centers called edge locations. When a user requests content that you're serving with CloudFront, the request is routed to the edge location that provides the lowest latency (time delay), so that content is delivered with the best possible performance.
  - If the content is already in the edge location with the lowest latency, CloudFront delivers it immediately.
  - If the content is not in that edge location, CloudFront retrieves it from an origin that you've defined—such as an Amazon S3 bucket, a MediaPackage channel, or an HTTP server (for example, a web server) that you have identified as the source for the definitive version of your content.
- CloudFront speeds up the distribution of your content by routing each user request through the AWS backbone network to the edge location that can best serve your content. Typically, this is a CloudFront edge server that provides the fastest delivery to the viewer. Using the AWS network dramatically reduces the number of networks that your users' requests must pass through, which improves performance. Users get lower latency—the time it takes to load the first byte of the file—and higher data transfer rates.
- You also get increased reliability and availability because copies of your files (also known as objects) are now held (or cached) in multiple edge locations around the world.

## CloudFront Workflow
![image](https://github.com/user-attachments/assets/5ea0955b-9437-4039-9957-7cb167ff9558)

Amazon CloudFront is a **Content Delivery Network (CDN)** service that speeds up the delivery of your web content by caching it in multiple locations worldwide. This reduces latency, improves performance, and helps efficiently serve users from the nearest location.

### **Step-by-Step CloudFront Workflow**
The image illustrates how CloudFront works in 5 key steps:

#### **1. Storing Content in the Origin Server**
   - The origin server is the main storage for your web content.
   - It can be **Amazon S3 (Simple Storage Service)** or an **HTTP server** (such as Apache, Nginx, or an EC2 instance running a web server).
   - You **upload objects** (files like images, videos, HTML, JavaScript, and CSS) to the origin server.

#### **2. Defining the Objects**
   - These objects are the actual content that users will request.
   - CloudFront can deliver static assets (images, stylesheets, JavaScript) and dynamic content (APIs, videos, and live streaming).
   - If stored in **Amazon S3**, you can make them **public** or **private** (using signed URLs/cookies for restricted access).

#### **3. Creating a CloudFront Distribution**
   - A **distribution** is a CloudFront setup that connects your origin (Amazon S3/HTTP server) to CloudFront’s edge locations.
   - You configure:
     - **Origin URL** (where CloudFront fetches data from)
     - **Caching policies** (how long content is stored in edge locations)
     - **Security settings** (encryption, signed URLs, authentication)
     - **Logging options** (to track user access)

#### **4. CloudFront Assigns a Unique URL**
   - After creating a distribution, CloudFront generates a **unique domain name** (e.g., `d111111abcdef8.cloudfront.net`).
   - You can use this domain to serve content or map it to your **custom domain** (e.g., `www.example.com`).

#### **5. Serving Content from Edge Locations**
   - CloudFront caches copies of your content at **edge locations** (servers worldwide).
   - When a user requests a file:
     - If it’s **cached** in the nearest edge location → CloudFront delivers it instantly.
     - If it’s **not cached** → CloudFront fetches it from the origin, caches it, and serves the user.
   - This process reduces latency, improves load times, and saves bandwidth.

---

### **Why Use Amazon CloudFront?**
🔹 **Faster Load Times** → Content is served from the nearest location.  
🔹 **Reduces Server Load** → Edge locations handle most traffic, reducing requests to your origin.  
🔹 **Highly Scalable** → Automatically handles millions of requests per second.  
🔹 **Security Features** → Supports HTTPS, signed URLs, AWS Shield (DDoS protection), and IAM-based access controls.  
🔹 **Cost-Effective** → You pay for what you use, and caching reduces data transfer costs.  

---

### **Advanced Configuration**
🔹 **Custom Domain Name (CNAMEs)** → Use your own domain (`www.example.com`) instead of the CloudFront URL.  
🔹 **Cache Expiration** → Set expiration times for content in edge locations (default: 24 hours).  
🔹 **Private Content** → Use signed URLs and signed cookies to restrict access.  
🔹 **Origin Shield** → Adds another caching layer to further reduce load on the origin.  
🔹 **AWS WAF Integration** → Protect against common web attacks.  

---

### **Features of AWS CloudFront**
1. **Low Latency & High Performance** – Uses edge locations to cache content closer to users.
2. **Security** – Integrates with AWS Shield, WAF, and ACM for DDoS protection and secure HTTPS connections.
3. **Customizable Content Delivery** – Supports dynamic and static content, video streaming, and API acceleration.
4. **Integration with AWS Services** – Works with S3, EC2, Elastic Load Balancer (ELB), and Lambda@Edge.
5. **Access Control** – Restricts access using signed URLs, signed cookies, and geo-restrictions.
6. **Logging & Monitoring** – Provides real-time logs and integrates with CloudWatch and AWS X-Ray for insights.
7. **Cost Optimization** – Offers price classes to optimize costs based on performance needs.

---

## **Understanding CloudFront with an S3 Bucket**
### **Scenario:**
- An **S3 bucket** is created in a US region.
- A **user from the Asia-Pacific region** attempts to access an object stored in the S3 bucket.
- Direct access to the S3 bucket introduces **latency** due to the geographic distance.
- **CloudFront** mitigates latency by caching content at an edge location near the user, ensuring faster response times.

### **How CloudFront Works with S3:**
1. A user requests a static file stored in an **S3 bucket**.
2. CloudFront checks if the requested file is **cached** in the nearest edge location.
3. If the file is **cached**, CloudFront serves it immediately.
4. If the file is **not cached**, CloudFront retrieves it from S3, caches it at the edge location, and delivers it to the user.
5. Subsequent requests for the same file are served from the edge location, reducing latency.

---

### **AWS CloudFront Pricing**
AWS CloudFront pricing is based on:
1. **Data Transfer Out** – Charged per GB based on region.
2. **Requests Pricing** – HTTP/HTTPS requests per 10,000 requests.
3. **Invalidation Requests** – First 1,000 paths free; additional at $0.005 per path.
4. **Function Execution Costs** – Lambda@Edge execution and processing charges.

To estimate costs, use the **AWS Pricing Calculator**.

---


