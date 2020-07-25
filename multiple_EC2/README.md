# playing-with-ansible

terraform init
terraform apply

Error: Missing resource instance key

  on outputs.tf line 6, in output "http_server_public_dns":
   6:   value = aws_instance.http_server.public_dns

Because aws_instance.http_server has "count" set, its attributes must be
accessed on specific instances.

For example, to correlate with indices of a referring resource, use:
    aws_instance.http_server[count.index]

Change at outputs.tf:
output "http_server_public_dns" {
  value = aws_instance.http_server.*.public_dns
}

need to set env for the AWS key
export AWS_ACCESS_KEY_ID=xxxxx
export AWS_SECRET_ACCESS_KEY=xxxxxx
