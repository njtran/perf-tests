CL2_IMAGE_NAME ?= aws/clusterloader2
CL2_TAG ?= latest
CL2_AWS_ACCOUNT_ID ?= $(shell aws sts get-caller-identity --query Account --output text)
CL2_AWS_REGION ?= us-west-2
CL2_DISTRIBUTION = linux-amd64
CL2_S3_BUCKET = s3://eks-prow
CL2_S3_PREFIX ?= $(CL2_S3_BUCKET)/bin/aws-k8s-tester
CL2_S3_PATH ?= $(CL2_S3_PREFIX)/clusterloader2-$(CL2_TAG)-$(CL2_DISTRIBUTION)
CL2_ECR_HOST ?= amazonaws.com

WHAT ?= clusterloader2
TARGETS ?= $(shell uname | awk '{print tolower($0)}')

docker-build:
    docker build -t $(CL2_IMG_NAME):$(CL2_TAG) --build-arg RELEASE_VERSION=$(CL2_TAG) .
    @if [ ! -f "./tmp/clusterloader2"]
    docker tag $(CL2_IMG_NAME):$(CL2_TAG) $(CL2_AWS_ACCOUNT_ID).dkr.ecr.$(CL2_AWS_REGION).$(CL2_ECR_HOST)/$(CL2_IMG_NAME):$(CL2_TAG)
    docker run --rm -it $(CL2_IMAGE_NAME):$(CL2_TAG) aws --version

docker-push:
    eval $$(aws ecr get-login --registry-ids $(CL2_AWS_ACCOUNT_ID) --no-include-email --region $(CL2_AWS_REGION))
    docker push $(CL2_AWS_ACCOUNT_ID).dkr.ecr.$(CL2_AWS_REGION).$(CL2_ECR_HOST)/$(CL2_IMG_NAME):$(CL2_TAG);
