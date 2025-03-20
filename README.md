# CloudFront (Securely deliver content with low latency and high transfer speeds)
- Amazon CloudFront is a fast content delivery network (CDN) service that securely delivers data, videos, applications, and APIs to customers globally with low latency and high transfer speeds.
- Amazon CloudFront is a web service that speeds up distribution of your static and dynamic web content, such as .html, .css, .js, and image files, to your users. CloudFront delivers your content through a worldwide network of data centers called edge locations. When a user requests content that you're serving with CloudFront, the request is routed to the edge location that provides the lowest latency (time delay), so that content is delivered with the best possible performance.
  - If the content is already in the edge location with the lowest latency, CloudFront delivers it immediately.
  - If the content is not in that edge location, CloudFront retrieves it from an origin that you've defined—such as an Amazon S3 bucket, a MediaPackage channel, or an HTTP server (for example, a web server) that you have identified as the source for the definitive version of your content.
- CloudFront speeds up the distribution of your content by routing each user request through the AWS backbone network to the edge location that can best serve your content. Typically, this is a CloudFront edge server that provides the fastest delivery to the viewer. Using the AWS network dramatically reduces the number of networks that your users' requests must pass through, which improves performance. Users get lower latency—the time it takes to load the first byte of the file—and higher data transfer rates.
- You also get increased reliability and availability because copies of your files (also known as objects) are now held (or cached) in multiple edge locations around the world.

## Set-Up of CloudFront to Deliver Content
![image](https://github.com/user-attachments/assets/5ea0955b-9437-4039-9957-7cb167ff9558)





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




---

### **AWS CloudFront Pricing**
AWS CloudFront pricing is based on:
1. **Data Transfer Out** – Charged per GB based on region.
2. **Requests Pricing** – HTTP/HTTPS requests per 10,000 requests.
3. **Invalidation Requests** – First 1,000 paths free; additional at $0.005 per path.
4. **Function Execution Costs** – Lambda@Edge execution and processing charges.

To estimate costs, use the **AWS Pricing Calculator**.

---

### **Terraform for CloudFront**
Terraform can automate CloudFront distribution creation. Below is an example:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-cloudfront-bucket"
}

resource "aws_cloudfront_origin_access_control" "oac" {
  name                              = "my-oac"
  origin_access_control_origin_type = "s3"
  signing_behavior                  = "always"
  signing_protocol                  = "sigv4"
}

resource "aws_cloudfront_distribution" "my_distribution" {
  origin {
    domain_name              = aws_s3_bucket.my_bucket.bucket_regional_domain_name
    origin_id                = "S3Origin"
    origin_access_control_id = aws_cloudfront_origin_access_control.oac.id
  }

  enabled         = true
  default_root_object = "index.html"

  default_cache_behavior {
    target_origin_id       = "S3Origin"
    viewer_protocol_policy = "redirect-to-https"
    allowed_methods        = ["GET", "HEAD"]
    cached_methods         = ["GET", "HEAD"]
    forwarded_values {
      query_string = false
      cookies {
        forward = "none"
      }
    }
  }

  viewer_certificate {
    cloudfront_default_certificate = true
  }
}
```

#### **Steps to Deploy Using Terraform**
1. Save the file as `main.tf`.
2. Run:
   ```sh
   terraform init
   terraform apply -auto-approve
   ```
3. CloudFront distribution will be created.

---
