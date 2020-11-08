# terraform-aws-lambda-scheduler-stop-start

Stop and start instance, rds resources and autoscaling groups with lambda function.

## Features

*  ec2 instances scheduling
*  rds instances scheduling
*  autoscalings scheduling

## Usage

```hcl
module "lambda-scheduler-stop" {
  source  = "app.terraform.io/moayadi/lambda-scheduler-stop-start/aws"
  version = "2.10.0"
  name                           = "${var.TFC_WORKSPACE_NAME}_ec2_stop"
  cloudwatch_schedule_expression = "cron(00 20 * * ? *)"
  schedule_action                = "stop"
  ec2_schedule                   = "true"
  rds_schedule                   = "false"
  autoscaling_schedule           = "false"
  resources_tag                  = {
    key   = "Environment"
    value = "dev"
  }
  tags = local.common_tags
}

module "lambda-scheduler-start" {
  source  = "app.terraform.io/moayadi/lambda-scheduler-stop-start/aws"
  version = "2.10.0"
  name                           = "${var.TFC_WORKSPACE_NAME}_ec2_start"
  cloudwatch_schedule_expression = "cron(00 20 * * ? *)"
  schedule_action                = "stop"
  ec2_schedule                   = "true"
  rds_schedule                   = "false"
  autoscaling_schedule           = "false"
  resources_tag                  = {
    key   = "Environment"
    value = "dev"
  }
  tags = local.common_tags
}
```

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|----------|
| name | Define name to use for lambda function, cloudwatch event and iam role | string | n/a | yes |
| custom_iam_role_arn | Custom IAM role arn for the scheduling lambda | string | null | no |
| kms_key_arn | The ARN for the KMS encryption key. If this configuration is not provided when environment variables are in use, AWS Lambda uses a default service key | string | null | no |
| aws_regions | A list of one or more aws regions where the lambda will be apply, default use the current region | list | null | no |
| cloudwatch_schedule_expression | The scheduling expression | string | `"cron(0 22 ? * MON-FRI *)"` | yes |
| schedule_action | Define schedule action to apply on resources | string | `"stop"` | yes |
| resources_tag | Set the tag use for identify resources to stop or start | map | { tostop = "true" } | yes |
| autoscaling_schedule | Enable scheduling on autoscaling resources | string | `"false"` | no |
| spot_schedule | Enable scheduling on spot instance resources | string | `"false"` | no |
| ec2_schedule | Enable scheduling on ec2 instance resources | string | `"false"` | no |
| rds_schedule | Enable scheduling on rds resources | string | `"false"` | no |
| cloudwatch_alarm_schedule | Enable scheduleding on cloudwatch alarm resources | string | `"false"` | no |

## Outputs

| Name | Description |
|------|-------------|
| lambda_iam_role_arn | The ARN of the IAM role used by Lambda function |
| lambda_iam_role_name | The name of the IAM role used by Lambda function |
| scheduler_lambda_arn | The ARN of the Lambda function |
| scheduler_lambda_name | The name of the Lambda function |
| scheduler_lambda_invoke_arn | The ARN to be used for invoking Lambda function from API Gateway |
| scheduler_lambda_function_last_modified | The date Lambda function was last modified |
| scheduler_lambda_function_version | Latest published version of your Lambda function |
| scheduler_log_group_name | The name of the scheduler log group |
| scheduler_log_group_arn | The Amazon Resource Name (ARN) specifying the log group |
