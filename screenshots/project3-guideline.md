# Project 3

## Create resource AWS

###  S3 

![s3.console.aws.amazon.com_s3_buckets_awsbucketproject3_region=us-east-1&tab=permissions](project3-guideline.assets/s3.console.aws.amazon.com_s3_buckets_awsbucketproject3_region=us-east-1&tab=permissions.png)
- Bucket policy

```json

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1625306057759",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::awsbucketproject3"
        }
    ]
}
```
- Cross-origin resource sharing (CORS)
```json
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "POST",
            "GET",
            "PUT",
            "DELETE",
            "HEAD"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": []
    }
]
```
### Cluster
- IAM cluster role `eksrole`

![image-20231108095959571](project3-guideline.assets/image-20231108100031563.png)
- Create cluster

![image-20231108101622874](project3-guideline.assets/image-20231108101622874.png)

- Result

![image-20231108095841915](project3-guideline.assets/image-20231108095841915.png)

![image-20231108095850313](project3-guideline.assets/image-20231108095850313.png)



### Node Group

- Create node Group
- create role first `udacity_nodegrop`
- `udacity_nodegrop` cluster role

![image-20231108095932288](project3-guideline.assets/image-20231108095932288.png)

- create node group

![image-20231108101459756](project3-guideline.assets/image-20231108101459756.png)

Note: config như này không dễ bị lỗi 403

![image-20231108101531973](project3-guideline.assets/image-20231108101531973.png)



###  postgres db

version < 13.7

- create rds

![image-20231108102020797](project3-guideline.assets/image-20231108102020797.png)
- result

![image-20231108100052638](project3-guideline.assets/image-20231108100052638.png)

![image-20231108100120082](project3-guideline.assets/image-20231108100120082.png)

## Source code

- Link Github: [nhonvo/project3 (github.com)](https://github.com/nhonvo/project3)

- Remember credential and session.



### Step 1: build source in local

- udagram-api-feed
- udagram-api-user
- udagram-frontend
- udagram-reverseproxy

`npm install -f`

### Step 2: Docker

- build docker image 

```
docker-compose -f docker-compose-build.yaml build --parallel
```

- smoke test in local(optional)

- Login docker at local `docker login`
- Push images to docker hubs
   -  `docker tag username/image:tag`
   - `docker push username/image:tag` `

- 