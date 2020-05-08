## EKS Architecture

![EKS Arch](images/eks-arch.png)

## Creating EKS Cluster from Local Mac 

### Prerequisite

- Install kubectl
- Install aws cli
- Setup AWS Account and Region (I am deploying on us-west-2) in bash profile
```
export ACCOUNT_ID=$(aws sts get-caller-identity --output text --query Account)
export AWS_REGION=us-west-2
echo "export ACCOUNT_ID=${ACCOUNT_ID}" | tee -a ~/.bash_profile
echo "export AWS_REGION=${AWS_REGION}" | tee -a ~/.bash_profile
aws configure set default.region ${AWS_REGION}
aws configure get default.region
```
- Make sure you have __IAM Admin role__ for AWS account you are running aws cli from
- Set up ssh
  - create ssh keys if not already
  ```
  ssh-keygen
  ```
  - Upload the public key to your EC2 region:
  ```
  aws ec2 import-key-pair --key-name "eksworkshop" --public-key-material fileb://~/.ssh/id_rsa.pub
  ```

- Create a CMK for the EKS cluster to use when encrypting your Kubernetes secrets and set it up in bash profile
  ```
  aws kms create-alias --alias-name alias/eksworkshop --target-key-id $(aws kms create-key --query KeyMetadata.Arn --output text)
  export MASTER_ARN=$(aws kms describe-key --key-id alias/eksworkshop --query KeyMetadata.Arn --output text)
  echo "export MASTER_ARN=${MASTER_ARN}" | tee -a ~/.bash_profile
  ```

### eksctl

- Install eksctl
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

sudo mv -v /tmp/eksctl /usr/local/bin
```
Confirm the eksctl command works:
```
eksctl version
```

- Launch EKS

Create a file __eksworkshop.yaml__ as in this folder

Then Run command to launch cluster
```
eksctl create cluster -f eksworkshop.yaml
```
- Test Cluster

```
kubectl get nodes
```

Export the Worker Role Name for use throughout the workshop
```
STACK_NAME=$(eksctl get nodegroup --cluster eksworkshop-eksctl -o json | jq -r '.[].StackName')
ROLE_NAME=$(aws cloudformation describe-stack-resources --stack-name $STACK_NAME | jq -r '.StackResources[] | select(.ResourceType=="AWS::IAM::Role") | .PhysicalResourceId')
echo "export ROLE_NAME=${ROLE_NAME}" | tee -a ~/.bash_profile
```