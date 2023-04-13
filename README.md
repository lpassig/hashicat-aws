# hashicat-aws
Hashicat: A terraform built application for use in Hashicorp workshops

Includes "Meow World" website.

[![infrastructure-tests](https://github.com/hashicorp/hashicat-aws/actions/workflows/infrastructure-tests.yml/badge.svg)](https://github.com/hashicorp/hashicat-aws/actions/workflows/infrastructure-tests.yml)



## Test Continuous Validation via a HTTP Status Check (embedded in main.tf)

<pre><code>#Introduce Postcondition (HTTP Status Check)

data "http" "hashicat-web" {
  depends_on = [null_resource.configure-cat-app]
  url = "http://${aws_eip.hashicat.public_dns}"

  lifecycle {
    postcondition {
      condition     = contains([200, 201, 204], self.status_code)
      error_message = "Status code invalid"
    }
  }
}</pre></code>

### How to
After deployment and initial check, delete html folder by connecting using SSH and the public IP (in the AWS Console)  under in "var/www" via command: <pre><code>sudo rm -rf html</pre></code>

### Diagram
<img width="1043" alt="Validation" src="https://user-images.githubusercontent.com/14903823/231701046-599244e1-7895-40e2-94de-c7a1701ce722.png">

## Test Drift via the AWS Console

Login to AWS Console and *add a new Tag* of your choice on the provisioned infrastructure. Wait for TFC to recognize the drift and react appropriatly by either : 

Accept the new settings and update your state by running a "refresh-only plan" **OR** Overwrite these changes by starting a new run. 

### Diagram
<img width="854" alt="Drift" src="https://user-images.githubusercontent.com/14903823/231703106-1c1bbe12-6aec-44ae-a885-997e0ba69d44.png">




