# AWS-CDK GitHub Actions

AWS-CDK GitHub Actions allow you to run `cdk deploy` and `cdk diff` and ... on your pull requests to help you review.

## Example usage

```yaml
on: [push]

jobs:
  aws_cdk:
    runs-on: ubuntu-latest
    steps:

      - name: cdk diff
        uses: youyo/aws-cdk-github-actions@master
        with:
          cdk_subcommand: 'diff'
          actions_comment: true
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_DEFAULT_REGION: 'ap-northeast-1'

      - name: cdk deploy
        uses: youyo/aws-cdk-github-actions@master
        with:
          cdk_subcommand: 'deploy'
          cdk_stack: 'stack1'
          actions_comment: false
          args: '--require-approval never'
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_DEFAULT_REGION: 'ap-northeast-1'

      - name: cdk synth
        uses: youyo/aws-cdk-github-actions@master
        with:
          cdk_subcommand: 'synth'
          cdk_version: '1.16.2'
          working_dir: 'src'
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_DEFAULT_REGION: 'ap-northeast-1'
```

### Can I take a assume-role?

If you use assume-role, we recommended using awscredswrap!  
See: https://github.com/marketplace/actions/aws-assume-role-github-actions#use-as-github-actions

```yaml
on: [push]

jobs:
  aws_cdk:
    runs-on: ubuntu-latest
    steps:
      - name: Assume Role
        uses: youyo/awscredswrap@master
        with:
          role_arn: ${{ secrets.ROLE_ARN }}
          duration_seconds: 3600
          role_session_name: 'awscredswrap@GitHubActions'
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_DEFAULT_REGION: 'ap-northeast-1'

      - name: cdk diff
        uses: youyo/aws-cdk-github-actions@master
        with:
          cdk_subcommand: 'diff'
```

## Inputs

- `cdk_subcommand` **Required** AWS CDK subcommand to execute.
- `cdk_version` AWS CDK version to install. (default: 'latest')
- `cdk_stack` AWS CDK stack name to execute. (default: '*')
- `working_dir` AWS CDK working directory. (default: '.')
- `actions_comment` Whether or not to comment on pull requests. (default: true)

## ENV

- `AWS_ACCESS_KEY_ID` **Required**
- `AWS_SECRET_ACCESS_KEY` **Required**

Recommended to get `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` from secrets.

## License

[MIT](LICENSE)

## Author

[youyo](https://github.com/youyo)
