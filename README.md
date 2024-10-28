## これなに？

[aws-autostop](https://github.com/yasutaka-kato/aws-autostop)というOSSを改造して、ついてるタグにおうじて自動的に起動/停止するLambdaです

## デプロイ方法

CloudShellから以下のコマンドを実行する

```
$ git clone https://github.com/yasutaka-kato/aws-autostop
$ cd aws-autostop
$ sam build
$ sam deploy --guided
  Stack Name [sam-app]: aws-autostop
  AWS Region [ap-northeast-1]: 
  Parameter TimeZone [UTC]: Asia/Tokyo
  Parameter SlackWebhookUrl []: 
  Confirm changes before deploy [y/N]: 
  Allow SAM CLI IAM role creation [Y/n]: 
  #Preserves the state of previously provisioned resources when an operation fails
  Disable rollback [y/N]: 
  Save arguments to configuration file [Y/n]: 
  SAM configuration file [samconfig.toml]: 
  SAM configuration environment [default]: 
```

消すときは同じディレクトリから

```
$ sam delete
```

## 分かってる問題点

(今のところなし)

# 以下は元のドキュメントです

# AWS AutoStop

Stop/start resources automatically depends on tag.

## Supported resources

- EC2 instance
- EC2 instance managed by AutoScaling Group
  - Control by scaling to 0 (when stop) or to 1 (when start).
- RDS cluster/instance

It might fail to start/stop resources in progress or invalid state.

## Prerequisite

- [AWS SAM CLI](https://docs.aws.amazon.com/ja_jp/serverless-application-model/latest/developerguide/install-sam-cli.html)

## Deploy

```shell
sam build (--use-container)
sam deploy (--guided)
```

### Parameters

|Name|Default|Description|
|:--|:--|:--|
|Timezone|"`UTC`"|Timezone to evaluate time configuration. (example: `Asia/Tokyo`)|
|SlackWebhookUrl|(none)|Slack Webhook URL to notify. (can be ommitted)|

## How to use

Add tags below to resources.

|Key|Format|Description|
|:--|:--|:--|
|`Auto:StartAt` or `auto:start-at`|`[<dayOfWeeks>:]<hours>`|Time configuration to start resource|
|`Auto:StopAt` or `auto:stop-at`|`[<dayOfWeeks>:]<hours>`|Time configuration to stop resource|

### Example

|Example|Description|
|:--|:--|
|`21`|21:00|
|`0-23`|every hours|
|`sun:0-2`|0:00, 1:00 and 2:00 on sunday|
|`6 12 18`|6:00, 12:00 and 18:00 everyday|
