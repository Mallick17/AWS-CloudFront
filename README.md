# **Terraform for CloudFront**
Terraform can automate CloudFront distribution creation. Below is an example:

## Script to Create CloudFront as S3 Origin
```hcl
provider "aws" {
  region = "ap-south-1"
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "mallick-static-site-site"
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

  enabled             = true
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

  restrictions {
    geo_restriction {
      restriction_type = "none"
    }
  }

  viewer_certificate {
    cloudfront_default_certificate = true
  }
}
```

<details>
   <summary>State File of the Created and Running Resource</summary>

root@ip-172-31-37-110:~/tf-cloudfront# cat terraform.tfstate
```hcl
{
  "version": 4,
  "terraform_version": "1.11.2",
  "serial": 17,
  "lineage": "7ed73276-fab1-94e4-0276-232cad1c6343",
  "outputs": {},
  "resources": [
    {
      "mode": "managed",
      "type": "aws_cloudfront_distribution",
      "name": "my_distribution",
      "provider": "provider[\"registry.terraform.io/hashicorp/aws\"]",
      "instances": [
        {
          "schema_version": 1,
          "attributes": {
            "aliases": null,
            "arn": "arn:aws:cloudfront::339713104321:distribution/E1HJW91BESZVRX",
            "caller_reference": "terraform-20250321072328494700000001",
            "comment": null,
            "continuous_deployment_policy_id": "",
            "custom_error_response": [],
            "default_cache_behavior": [
              {
                "allowed_methods": [
                  "GET",
                  "HEAD"
                ],
                "cache_policy_id": "",
                "cached_methods": [
                  "GET",
                  "HEAD"
                ],
                "compress": false,
                "default_ttl": 0,
                "field_level_encryption_id": "",
                "forwarded_values": [
                  {
                    "cookies": [
                      {
                        "forward": "none",
                        "whitelisted_names": []
                      }
                    ],
                    "headers": [],
                    "query_string": false,
                    "query_string_cache_keys": []
                  }
                ],
                "function_association": [],
                "grpc_config": [
                  {
                    "enabled": false
                  }
                ],
                "lambda_function_association": [],
                "max_ttl": 0,
                "min_ttl": 0,
                "origin_request_policy_id": "",
                "realtime_log_config_arn": "",
                "response_headers_policy_id": "",
                "smooth_streaming": false,
                "target_origin_id": "S3Origin",
                "trusted_key_groups": [],
                "trusted_signers": [],
                "viewer_protocol_policy": "redirect-to-https"
              }
            ],
            "default_root_object": "index.html",
            "domain_name": "d2p3jca5q3a57g.cloudfront.net",
            "enabled": true,
            "etag": "E3QPYCS434NFK9",
            "hosted_zone_id": "Z2FDTNDATAQYW2",
            "http_version": "http2",
            "id": "E1HJW91BESZVRX",
            "in_progress_validation_batches": 0,
            "is_ipv6_enabled": false,
            "last_modified_time": "2025-03-21 07:23:28.652 +0000 UTC",
            "logging_config": [],
            "ordered_cache_behavior": [],
            "origin": [
              {
                "connection_attempts": 3,
                "connection_timeout": 10,
                "custom_header": [],
                "custom_origin_config": [],
                "domain_name": "mallick-static-site-site.s3.ap-south-1.amazonaws.com",
                "origin_access_control_id": "EU0XO07QTDGCV",
                "origin_id": "S3Origin",
                "origin_path": "",
                "origin_shield": [],
                "s3_origin_config": [],
                "vpc_origin_config": []
              }
            ],
            "origin_group": [],
            "price_class": "PriceClass_All",
            "restrictions": [
              {
                "geo_restriction": [
                  {
                    "locations": [],
                    "restriction_type": "none"
                  }
                ]
              }
            ],
            "retain_on_delete": false,
            "staging": false,
            "status": "Deployed",
            "tags": null,
            "tags_all": {},
            "trusted_key_groups": [
              {
                "enabled": false,
                "items": []
              }
            ],
            "trusted_signers": [
              {
                "enabled": false,
                "items": []
              }
            ],
            "viewer_certificate": [
              {
                "acm_certificate_arn": "",
                "cloudfront_default_certificate": true,
                "iam_certificate_id": "",
                "minimum_protocol_version": "TLSv1",
                "ssl_support_method": ""
              }
            ],
            "wait_for_deployment": true,
            "web_acl_id": ""
          },
          "sensitive_attributes": [],
          "private": "eyJzY2hlbWFfdmVyc2lvbiI6IjEifQ==",
          "dependencies": [
            "aws_cloudfront_origin_access_control.oac",
            "aws_s3_bucket.my_bucket"
          ]
        }
      ]
    },
    {
      "mode": "managed",
      "type": "aws_cloudfront_origin_access_control",
      "name": "oac",
      "provider": "provider[\"registry.terraform.io/hashicorp/aws\"]",
      "instances": [
        {
          "schema_version": 0,
          "attributes": {
            "arn": "arn:aws:cloudfront::339713104321:origin-access-control/EU0XO07QTDGCV",
            "description": "Managed by Terraform",
            "etag": "ETVPDKIKX0DER",
            "id": "EU0XO07QTDGCV",
            "name": "my-oac",
            "origin_access_control_origin_type": "s3",
            "signing_behavior": "always",
            "signing_protocol": "sigv4"
          },
          "sensitive_attributes": [],
          "private": "bnVsbA=="
        }
      ]
    },
    {
      "mode": "managed",
      "type": "aws_s3_bucket",
      "name": "my_bucket",
      "provider": "provider[\"registry.terraform.io/hashicorp/aws\"]",
      "instances": [
        {
          "schema_version": 0,
          "attributes": {
            "acceleration_status": "",
            "acl": null,
            "arn": "arn:aws:s3:::mallick-static-site-site",
            "bucket": "mallick-static-site-site",
            "bucket_domain_name": "mallick-static-site-site.s3.amazonaws.com",
            "bucket_prefix": "",
            "bucket_regional_domain_name": "mallick-static-site-site.s3.ap-south-1.amazonaws.com",
            "cors_rule": [],
            "force_destroy": false,
            "grant": [
              {
                "id": "852b3ddbb7b9294824a2ececf0387cf0a9f734163b58d96287cd6e4d45fd6ab0",
                "permissions": [
                  "FULL_CONTROL"
                ],
                "type": "CanonicalUser",
                "uri": ""
              }
            ],
            "hosted_zone_id": "Z11RGJOFQNVJUP",
            "id": "mallick-static-site-site",
            "lifecycle_rule": [],
            "logging": [],
            "object_lock_configuration": [],
            "object_lock_enabled": false,
            "policy": "",
            "region": "ap-south-1",
            "replication_configuration": [],
            "request_payer": "BucketOwner",
            "server_side_encryption_configuration": [
              {
                "rule": [
                  {
                    "apply_server_side_encryption_by_default": [
                      {
                        "kms_master_key_id": "",
                        "sse_algorithm": "AES256"
                      }
                    ],
                    "bucket_key_enabled": false
                  }
                ]
              }
            ],
            "tags": null,
            "tags_all": {},
            "timeouts": null,
            "versioning": [
              {
                "enabled": false,
                "mfa_delete": false
              }
            ],
            "website": [],
            "website_domain": null,
            "website_endpoint": null
          },
          "sensitive_attributes": [],
          "private": "eyJlMmJmYjczMC1lY2FhLTExZTYtOGY4OC0zNDM2M2JjN2M0YzAiOnsiY3JlYXRlIjoxMjAwMDAwMDAwMDAwLCJkZWxldGUiOjM2MDAwMDAwMDAwMDAsInJlYWQiOjEyMDAwMDAwMDAwMDAsInVwZGF0ZSI6MTIwMDAwMDAwMDAwMH19"
        }
      ]
    }
  ],
  "check_results": null
}

```

</details>

- After `terraform destroy` command is given `terraform.tfstate.backup` file is created.

<details>
   <summary>terraform.tfstate.backup File of the Created and Running Resource</summary>

root@ip-172-31-37-110:~/tf-cloudfront# cat terraform.tfstate.backup
```hcl
{
  "version": 4,
  "terraform_version": "1.11.2",
  "serial": 9,
  "lineage": "7ed73276-fab1-94e4-0276-232cad1c6343",
  "outputs": {},
  "resources": [
    {
      "mode": "managed",
      "type": "aws_cloudfront_distribution",
      "name": "my_distribution",
      "provider": "provider[\"registry.terraform.io/hashicorp/aws\"]",
      "instances": [
        {
          "schema_version": 1,
          "attributes": {
            "aliases": null,
            "arn": "arn:aws:cloudfront::339713104321:distribution/E2U9Z284705EYC",
            "caller_reference": "terraform-20250321052615225200000001",
            "comment": null,
            "continuous_deployment_policy_id": "",
            "custom_error_response": [],
            "default_cache_behavior": [
              {
                "allowed_methods": [
                  "GET",
                  "HEAD"
                ],
                "cache_policy_id": "",
                "cached_methods": [
                  "GET",
                  "HEAD"
                ],
                "compress": false,
                "default_ttl": 0,
                "field_level_encryption_id": "",
                "forwarded_values": [
                  {
                    "cookies": [
                      {
                        "forward": "none",
                        "whitelisted_names": []
                      }
                    ],
                    "headers": [],
                    "query_string": false,
                    "query_string_cache_keys": []
                  }
                ],
                "function_association": [],
                "grpc_config": [
                  {
                    "enabled": false
                  }
                ],
                "lambda_function_association": [],
                "max_ttl": 0,
                "min_ttl": 0,
                "origin_request_policy_id": "",
                "realtime_log_config_arn": "",
                "response_headers_policy_id": "",
                "smooth_streaming": false,
                "target_origin_id": "S3Origin",
                "trusted_key_groups": [],
                "trusted_signers": [],
                "viewer_protocol_policy": "redirect-to-https"
              }
            ],
            "default_root_object": "index.html",
            "domain_name": "d3ma64atow6zys.cloudfront.net",
            "enabled": true,
            "etag": "E1PZ26F66FLG7A",
            "hosted_zone_id": "Z2FDTNDATAQYW2",
            "http_version": "http2",
            "id": "E2U9Z284705EYC",
            "in_progress_validation_batches": 0,
            "is_ipv6_enabled": false,
            "last_modified_time": "2025-03-21 05:26:15.385 +0000 UTC",
            "logging_config": [],
            "ordered_cache_behavior": [],
            "origin": [
              {
                "connection_attempts": 3,
                "connection_timeout": 10,
                "custom_header": [],
                "custom_origin_config": [],
                "domain_name": "mallick-static-site-site.s3.ap-south-1.amazonaws.com",
                "origin_access_control_id": "E2IJJQS2ZSO1YH",
                "origin_id": "S3Origin",
                "origin_path": "",
                "origin_shield": [],
                "s3_origin_config": [],
                "vpc_origin_config": []
              }
            ],
            "origin_group": [],
            "price_class": "PriceClass_All",
            "restrictions": [
              {
                "geo_restriction": [
                  {
                    "locations": [],
                    "restriction_type": "none"
                  }
                ]
              }
            ],
            "retain_on_delete": false,
            "staging": false,
            "status": "Deployed",
            "tags": null,
            "tags_all": {},
            "trusted_key_groups": [
              {
                "enabled": false,
                "items": []
              }
            ],
            "trusted_signers": [
              {
                "enabled": false,
                "items": []
              }
            ],
            "viewer_certificate": [
              {
                "acm_certificate_arn": "",
                "cloudfront_default_certificate": true,
                "iam_certificate_id": "",
                "minimum_protocol_version": "TLSv1",
                "ssl_support_method": ""
              }
            ],
            "wait_for_deployment": true,
            "web_acl_id": ""
          },
          "sensitive_attributes": [],
          "private": "eyJzY2hlbWFfdmVyc2lvbiI6IjEifQ==",
          "dependencies": [
            "aws_cloudfront_origin_access_control.oac",
            "aws_s3_bucket.my_bucket"
          ]
        }
      ]
    },
    {
      "mode": "managed",
      "type": "aws_cloudfront_origin_access_control",
      "name": "oac",
      "provider": "provider[\"registry.terraform.io/hashicorp/aws\"]",
      "instances": [
        {
          "schema_version": 0,
          "attributes": {
            "arn": "arn:aws:cloudfront::339713104321:origin-access-control/E2IJJQS2ZSO1YH",
            "description": "Managed by Terraform",
            "etag": "ETVPDKIKX0DER",
            "id": "E2IJJQS2ZSO1YH",
            "name": "my-oac",
            "origin_access_control_origin_type": "s3",
            "signing_behavior": "always",
            "signing_protocol": "sigv4"
          },
          "sensitive_attributes": [],
          "private": "bnVsbA=="
        }
      ]
    },
    {
      "mode": "managed",
      "type": "aws_s3_bucket",
      "name": "my_bucket",
      "provider": "provider[\"registry.terraform.io/hashicorp/aws\"]",
      "instances": [
        {
          "schema_version": 0,
          "attributes": {
            "acceleration_status": "",
            "acl": null,
            "arn": "arn:aws:s3:::mallick-static-site-site",
            "bucket": "mallick-static-site-site",
            "bucket_domain_name": "mallick-static-site-site.s3.amazonaws.com",
            "bucket_prefix": "",
            "bucket_regional_domain_name": "mallick-static-site-site.s3.ap-south-1.amazonaws.com",
            "cors_rule": [],
            "force_destroy": false,
            "grant": [
              {
                "id": "852b3ddbb7b9294824a2ececf0387cf0a9f734163b58d96287cd6e4d45fd6ab0",
                "permissions": [
                  "FULL_CONTROL"
                ],
                "type": "CanonicalUser",
                "uri": ""
              }
            ],
            "hosted_zone_id": "Z11RGJOFQNVJUP",
            "id": "mallick-static-site-site",
            "lifecycle_rule": [],
            "logging": [],
            "object_lock_configuration": [],
            "object_lock_enabled": false,
            "policy": "",
            "region": "ap-south-1",
            "replication_configuration": [],
            "request_payer": "BucketOwner",
            "server_side_encryption_configuration": [
              {
                "rule": [
                  {
                    "apply_server_side_encryption_by_default": [
                      {
                        "kms_master_key_id": "",
                        "sse_algorithm": "AES256"
                      }
                    ],
                    "bucket_key_enabled": false
                  }
                ]
              }
            ],
            "tags": null,
            "tags_all": {},
            "timeouts": null,
            "versioning": [
              {
                "enabled": false,
                "mfa_delete": false
              }
            ],
            "website": [],
            "website_domain": null,
            "website_endpoint": null
          },
          "sensitive_attributes": [],
          "private": "eyJlMmJmYjczMC1lY2FhLTExZTYtOGY4OC0zNDM2M2JjN2M0YzAiOnsiY3JlYXRlIjoxMjAwMDAwMDAwMDAwLCJkZWxldGUiOjM2MDAwMDAwMDAwMDAsInJlYWQiOjEyMDAwMDAwMDAwMDAsInVwZGF0ZSI6MTIwMDAwMDAwMDAwMH19"
        }
      ]
    }
  ],
  "check_results": null
}
```

</details>

