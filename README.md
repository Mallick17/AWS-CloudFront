# **Terraform for CloudFront**
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
