# Configure AWS Credential Profiles for GitHub Actions
This action uses the official `aws-actions/configure-aws-credentials@v2` action. This action only supports assuming roles via OIDC.

The official action is not sufficient for multiple account usage as it can only set one set of AWS environment variables at a time.

> The primary reason this action exists is to address using multiple profiles at the same time. Region defaults to `us-east-1`.

# Usage
```yaml
jobs:  
  test_new_action:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Repo
      uses: actions/checkout@v3

    - uses: getditto/configure-aws-profile-action@main
      with:
        role-arn: arn:aws:iam::<ACCOUNT_ID>:role/<ROLE_NAME>
        profile-name: test
        role-session-name: my-session-tag

    - uses: getditto/configure-aws-profile-action@main
      with:
        role-arn: arn:aws:iam::<ACCOUNT_ID>:role/<ROLE_NAME>
        profile-name: production
        region: us-east-2

    - run: aws s3 ls --profile test

    - run: aws s3 ls --profile production
```
