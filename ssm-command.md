## list process running inside all the ec2 in the AWS environment connect to SSM.

### SSM Command

```
for instance_id in $(aws ec2 describe-instances --query 'Reservations[].Instances[?State.Name==`running`].[InstanceId]' --output text); do
    echo "Running command on instance $instance_id"
    command_id=$(aws ssm send-command --instance-ids "$instance_id" --document-name "AWS-RunShellScript" --parameters "commands=['ps -auxwww']" --output text --query 'Command.CommandId')
    echo "Command ID: $command_id"
    status="Pending"
    while [[ "$status" == "Pending" || "$status" == "InProgress" ]]; do
    sleep 5
    status=$(aws ssm list-command-invocations --command-id "$command_id" --instance-id "$instance_id" --query 'CommandInvocations[].Status' --output text)
    done
    aws ssm list-command-invocations --command-id "$command_id" --details --query 'CommandInvocations[0].CommandPlugins[0]' --output text > $instance_id.txt
done 
```
