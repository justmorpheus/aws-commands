## Check if access key exist for the users from the list, also it will show the status of the key from the list of users. File keys.txt

```
while read user; do aws iam list-access-keys --user-name "$user" --query 'AccessKeyMetadata[].[AccessKeyId, Status]' --output text; done < keys.txt
```
