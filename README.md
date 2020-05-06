# Fargate

This deploys:
  * a shared ECS Cluster
  * a shared private Application Load balancer
  * and one or more Fargate Containers into the same cluster

This architecture is based off Nathan Peck's [AWS Cloudformation Fargate examples](https://github.com/nathanpeck/aws-cloudformation-fargate):
  * [Public + Private VPC + Cluster](https://github.com/nathanpeck/aws-cloudformation-fargate/blob/master/fargate-networking-stacks/public-private-vpc.yml)
  * [Private Subnet, Private Load Balancer + Fargate Container](https://github.com/nathanpeck/aws-cloudformation-fargate/blob/master/service-stacks/private-subnet-private-loadbalancer.yml)


## Usage

Deploy the Shared ECS Cluster and Load balancer stack:
```
aws cloudformation deploy \
  --stack-name=<fargate-cluster-stack-name> \
  --template-file cluster.yml \
  --parameter-overrides $(cat parameters/cluster/<aws-account> | egrep -v '^#') \
  --tags $(cat tags | egrep -v '^#')
```

Parameterise all the environment variables required by the container in fargate.yml

Deploy the Fargate Container stacks:
```
aws cloudformation deploy \
  --stack-name=<fargate-container-stack-name> \
  --template-file fargate.yml \
  --capabilities CAPABILITY_IAM \
  --parameter-overrides $(cat parameters/fargate/<aws-account> | egrep -v '^#') \
  --tags $(cat tags | egrep -v '^#')
```
