


### step 1



    aws cloudformation package \
    --template-file pipeline/other-accounts.yaml \
    --s3-bucket dev-bucket --output-template-file \
    pipeline/other-accounts-packaged.yaml --profile dev

    aws cloudformation deploy \
    --template-file pipeline/other-accounts-packaged.yaml \
    --stack-name DevOps-other-accounts --region eu-west-1 \
    --capabilities CAPABILITY_NAMED_IAM --profile dev --parameter-overrides CrossAccountCondition=false


### step 2




    aws cloudformation package \
    --template-file pipeline/root.yaml --s3-bucket dev-bucketops \
    --output-template-file pipeline/root-packaged.yaml --profile devops

    aws cloudformation deploy \
    --template-file pipeline/root-packaged.yaml  \
    --stack-name simple-pipeline --capabilities CAPABILITY_NAMED_IAM \
    --parameter-overrides Environment=dev RoleCondition=false \
    --region eu-west-1 --profile devops



### step 3

    aws cloudformation deploy \
    --template-file pipeline/other-accounts-packaged.yaml \
    --stack-name DevOps-other-accounts --region eu-west-1 \
    --capabilities CAPABILITY_NAMED_IAM --profile dev --parameter-overrides CrossAccountCondition=true

### step 4

    aws cloudformation deploy \
    --template-file pipeline/root-packaged.yaml  \
    --stack-name simple-pipeline --capabilities CAPABILITY_NAMED_IAM \
    --parameter-overrides Environment=dev RoleCondition=true \
    --region eu-west-1 --profile devops