## Check if list of users present in aws or not, used for post-validation
#!/bin/bash

# Read the user_list.txt file line by line
while read username; do
  # Fetch the IAM user using the AWS CLI
  aws iam get-user --user-name "$username" >/dev/null 2>&1

  # Check the exit status of the AWS CLI command
  if [ $? -eq 0 ]; then
    echo "User '$username' exists in IAM."
  else
    echo "User '$username' does not exist in IAM."
  fi
done <user_list.txt
