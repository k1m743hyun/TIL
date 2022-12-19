# Resource Meta-Arguments
- 모든 `resource`가 공통적으로 사용할 수 있는 *Arguments*


## 1. depends_on
- `depends_on`에 설정된 `resource`가 생성이 완료된 후에 해당 `resource`가 생성되기 시작


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

- Terraform에서 `resource` 간 dependency를 알 수 있는 경우에는 생략할 수 있음
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

- `depends_on`은 Terraform이 `resource` 간의 dependency를 알 수 없는 경우에만 사용하면 됨
- `depends_on` 사용 시 추후 관리를 위해 `resource` 간의 dependency가 필요한 이유를 함께 작성해두면 좋음


## 2. count


## 3. for_each


## 4. provider


## 5. lifecycle


# 출처
- [Terraform resource (2/2)](https://velog.io/@gentledev10/terraform-resource-2)