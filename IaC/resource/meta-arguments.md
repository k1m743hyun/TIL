# Resource Meta-Arguments
- 모든 resource가 공통적으로 사용할 수 있는 *Arguments*


## 1. depends_on
- `depends_on`에 설정된 resource가 생성이 완료된 후에 해당 resource가 생성되기 시작함


### example

- EC2가 생성되기 전에 S3 bucket이 생성 완료되어야 함
```
resource "aws_s3_bucket" "this" {
    bucket = var.bucket_name
}

resource "aws_instance" "this" {
    ami           = var.ami_id
    instance_type = var.instance_type

    depends_on = [
        aws_s3_bucket.this
    ]
}
```

- Terraform에서 resource 간 dependency를 알 수 있는 경우에는 생략할 수 있음
```
resource "aws_vpc" "this" {
    cidr_block = var.vpc_cidr
}

resource "aws_route_table" "this" {
    vpc_id = aws_vpc.id

    # depends_on = [
    #     aws_vpc.this
    # ]
}
```

- `depends_on`은 Terraform이 resource 간의 dependency를 알 수 없는 경우에만 사용하면 됨
- `depends_on` 사용 시 추후 관리를 위해 resource 간의 dependency가 필요한 이유를 함께 작성해두면 좋음


## 2. count
- resource block을 사용하여 resource를 생성하면 1개의 resource가 생성됨
- 동일한 resource block으로 여러 개의 동일한 resource type을 생성하고 싶을 때 `count`를 사용함


### example

- 3개의 IAM user를 생성함
```
resource "aws_iam_user" "this" {
    count = 3
    name  = "user_${count.index}"
}
```


### count object
- `count` argument를 사용하면 해당 resource block 안에서 `count` object를 어디서든 사용할 수 있음
- `count` argument를 사용하여 생성된 resource의 index 값을 `count` object를 통해 알 수 있음
    - `count.index`를 사용하여 resource의 index 값을 0부터 시작하여 (resource 생성 갯수 - 1)까지 차례대로 얻을 수 있음


### resource instance 참조
- `count` argument를 사용하여 생성된 resource를 참조하기 위해서는 아래 예시와 같이 `<TYPE>.<NAME>.[<INDEX>]` 문법을 사용하여 resource의 index를 명시해 참조할 수 있으며, 각 resource의 attribute도 참조할 수 있음
    - `aws_iam_user.this[0]`
    - `aws_iam_user.this[0].name`


#### 주의
- `count` argument의 값은 apply 전에 정해져 있어야 함
- 아래 예시와 같이 `count`에 apply 후에 정해지는 값이 있다면 error가 발생함
    - `count = aws_iam_group.this.name == "developer" : 1 : 2`


## 3. for_each


## 4. provider


## 5. lifecycle


# 출처
- [Terraform resource (2/2)](https://velog.io/@gentledev10/terraform-resource-2)