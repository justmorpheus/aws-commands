## Assume Role Function & Automation To Ser Keys

- A bash function that uses AWS STS to assume a role and export temporary credentials to be used for subsequent AWS CLI commands.

```
sts-assume-role() {
    role_arn=$1
    session_name=$2

    output=$(aws sts assume-role --role-arn $role_arn --role-session-name $session_name)

    export AWS_ACCESS_KEY_ID=$(echo $output | jq -r '.Credentials.AccessKeyId')
    export AWS_SECRET_ACCESS_KEY=$(echo $output | jq -r '.Credentials.SecretAccessKey')
    export AWS_SESSION_TOKEN=$(echo $output | jq -r '.Credentials.SessionToken')
}
```

- Bash function `sts-assume-role` called & the first as well as second parameters passed to the function, representing the role ARN and session name, respectively.

```
sts-assume-role <role_arn> <session_name>
```


- An AWS CLI command that returns details about the IAM user or role whose credentials are used to call the command.

```
aws sts get-caller-identity
```
