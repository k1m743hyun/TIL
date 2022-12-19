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


### 주의
- `count` argument의 값은 apply 전에 정해져 있어야 함
- 아래 예시와 같이 `count`에 apply 후에 정해지는 값이 있다면 error가 발생함
    - `count = aws_iam_group.this.name == "developer" : 1 : 2`


## 3. for_each
- `count`와 동일하게 한 개의 resource block으로 여러 개의 동일한 resource type을 생성하고자 할 때 사용함
    - `count`와 `for_each`는 resource block에서 동시에 사용할 수 없으므로 한 개만 선택해서 사용해야 함
- `for_each`는 map 또는 set을 값으로 가질 수 있으며, map 또는 set을 통해 전달된 값의 갯수만큼 resource를 생성함


### example

- set을 사용하여 3개의 IAM user를 생성하는 코드
```
resource "aws_iam_user" "this" {
    for_each = toset(["user1", "user2", "user3"])
    name     = each.key
}
```

- map을 사용하여 3개의 IAM user를 생성하는 코드
```
resource "aws_iam_user" "this" {
    for_each = {
        user1 = "tag1"
        user2 = "tag2"
        user3 = "tag3"
    }
    name     = each.key
    tags     = {
        tag = each.value
    }
}
```


### each object
- `for_each`를 사용하여 resource를 생성하면 `each` object를 resource block 내에서 사용할 수 있음
- `each` object를 통해 `each.key`, `each.value`를 사용할 수 있음
    - `each.key`는 map 사용 시에는 key 값을, set 사용 시에는 member 값을 의미함
    - `each.value`는 map 사용 시에는 value 값을, set 사용 시에는 `each.key`와 동일하게 member 값을 의미함


### resource instance 참조
- `for_each`를 사용하여 생성한 resource를 참조하기 위해서는 `<TYPE>.<NAME>[<KEY>]` 문법을 사용함
    - set을 사용한 resource 참조 시 `aws_iam_user.this["user2"].name`
    - map을 사용한 resource 참조 시 `aws_iam_user.this["user3"].name`


### 주의
- `count` argument와 마찬가지로 `for_each`의 값도 apply 전에 정해져 있어야 함
- 그렇지 않으면 apply 시 error가 발생하니 주의해야 함


## 4. provider
- AWS infrastructure를 구성하다보면 종종 다른 configuration으로 resource를 생성해야하는 경우가 생길 때 사용함
    - 예를 들면, 다른 region에 resource를 생성함


### example

- default configuration
```
provider "aws" {
    region = "us-east-1"
}
```

- alternate configuration
    - provider 당 default configuration은 반드시 1개만 선언할 수 있기 때문에 alias를 이용하여 alternate configuration을 생성함
```
provider "aws" {
    alias  = "seoul"
    region = "ap-northeast-2"
}
```

- default configuration을 사용하는 resorce 생성
    - provider를 선언하지 않고 생성함
```
resource "aws_instance" "this" {
    ami           = var.ami_id
    instance_type = var.instance_type
}
```

- alternate configuration을 사용하는 resource 생성
    - provider를 선언하여 원하는 configuration으로 생성함
```
resource "aws_instance" "this" {
    provider      = aws.seoul
    ami           = var.ami_id
    instance_type = var.instance_type
}
```


## 5. lifecycle


# 출처
- [Terraform resource (2/2)](https://velog.io/@gentledev10/terraform-resource-2)