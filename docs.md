# Stax

Create cloud stacks (aka stax) on AWS ([Amazon Web Services](aws.amazon.com)) quickly.

## The stax project is based on work from

    https://github.com/emmanuel/coreos-skydns-cloudformation
    and
    https://github.com/philcryer/coreos-aws-cloudformation
    and
    https://github.com/kelseyhightower/kubernetes-coreos

## Requirements

* Install [aws-cli](https://github.com/aws/aws-cli) (Universal Command Line Interface for Amazon Web Services) on your client.

```bash
apt-get install awscli  # Debian GNU/Linux, Ubuntu
brew install aws-cli    # Apple OS X (via Homebrew)
yum install awscli      # Red Hat Enterprise Linux (RHEL), Amazon Linux, Centos
```

* Configure the aws client with your AWS credentials.

```bash
aws configure
```

You will be prompted to enter your AWS access key and secret access key (this will write and store them in ~/.aws/credentials)

* Get AWS ssh private key for the 'coreoscluster01' keypair in s3, and then `ssh-add` it. Alternatively, use an existing key pair already on AWS, or generate your own on your AWS account
(you'll need to refer to this key in the create_stack command below).

* Install [curl](), to talk to the web and [jq](), to parse json output from the awscli

```bash
apt-get install jq curl   # Debian GNU/Linux, Ubuntu ???maybe test this
brew install jq curl      # Apple OS X (via Homebrew)
yum install jq curl       # Red Hat Enterprise Linux (RHEL), Amazon Linux, Centos ???test this
```

## Stax runs [CoreOS](https://coreos.com/) instances on AWS EC2

Configuration is handled by Cloud Formation, services installed via cloud-init are

* etcd
* fleet

## Usage

```bash
stax create     (create a new stack)
stax describe   (describe the stack)
stax update     (update the stack)
stax destroy    (destroy the stack)
```

## Create cluster

Copy the config file and salt to taste

```bash
cp config.json.example config.json
vi config.json
```

Run it

```bash
./stax create
```

## Access cluster

To access the hosts, get a PublicIP for one of the nodes from the AWS console (a work around for now), then ssh to it

```bash
ssh PUBLIC-IP -l core -i ~/.ssh/the-key-you-specified.pem
```

Once there you can see that it can see the other nodes via fleet

```bash
fleetctl list-machines
```

Ta... da.

## Tear it down

This will delete the stax, as well as the cloudformation scripts, security groups, etc that it created.

```bash
stax destroy
```
