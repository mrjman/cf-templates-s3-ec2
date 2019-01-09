# coa.cf-templates
### Overview
This repository contains CloudFormation templates for building the resources required for the Coachella website. The templates are broken out into subfolders with each corresponding to a part of the overall build. The sections below will provide more details on each.

##### WordPress
The WordPress architecture is based on the [AWS reference architecture](https://github.com/aws-samples/aws-refarch-wordpress) and provides a scalable, high availability WordPress server. Configuration files can be found in the `config-files` directory and are used during the CloudFormation setup of the WordPress server instances. All CloudFormation templates are stored in the templates directory and the `master.yml` template is meant to serve as the parent stack that creates all required nested stacks. When running the CloudFormation templates the following resources will be built:
* VPC (**optionally private subnets can be enabled**)
* VPC endpoint for S3 service (**optional, only built if private subnets enabled**)
* Security Groups for all resources: data, web, bastion, etc.
* S3 Bucket for media assets
* RDS database instance
* Bastion EC2 auto-scaling group for ssh access
* WordPress server EC2 Auto-scaling group
* Application Load balancer for Wordpress server instances
* CloudFront distribution for caching of WordPress site and media uploads (**optional**)
* Elasticache cluster for WordPress caching (**optional**)
* Route53 hosted zone and record set (**optional**)

##### Static Site
The static site templates are used to build the deployed site that is served to users. This site is depedent on the WordPress site to build the final statically generated assets. All CloudFormation templates are stored int he templates directory and the `master.yml` template is meant to serve as the parent stack that creates all required nested stacks. The CloudFormation templates set the prelaunch redirect rules on the site S3 bucket to ensure it is setup with the correct initial redirects. The redirect rules for post site launch can be found in the top level `redirects` directory in the `site_post_launch.xml` file. For reference the prelaunch redirects can also be found in the same directory in the `site_pre_launch.xml` file. When running the CloudFormation templates the following resources will be built:
* S3 bucket setup for website hosting to hold the final site files
* CloudFront distribution to provide https and caching of the S3 site files
* Lambda@Edge function to provide basic authentication (**optional**)
* Lambda@Edge function to provide security headers (**optional**)
* CodePipeline/CodeBuild for Continuous integration and deployment
* Lambda function for slack build notifications (**optional**)

##### Splash Page
The splash page CloudFormation templates are used to build the resources required to host the Coachella splash page. The CloudFormation templates set the prelaunch redirect rules on the splash page to ensure it is setup with the correct intial redirects. The redirect rules for post site launch can be found in the top level `redirects` directory in the `splash_post_launch.xml` file. For reference the prelaunch rediects can also be found in teh same directory in the `splash_pre_launch.xml` file. When running the CloudFormation templates the following resources will be built:
* S3 bucket setup for website hosting to hold the splash page site files
* CloudFront distribution to provide https and caching of the s3 site files
* Lambda@Edge function to provide security headers (**optional**)
