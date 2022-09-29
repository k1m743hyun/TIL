# Terraform CLI Commands


## Initializing Working Directories


### init


#### Usage

```
$ terraform init [options]
```

#### Options

- `-input=true`

- `-lock=false`

- `-lock-timeout=<duration>`

- `-no-color`

- `-upgrade`


## Provisioning Infrastructure


### plan


#### Usage

```
$ terraform plan [options]
```


#### Options

- `-refresh=false`

- `-replace=ADDRESS`

- `-target=ADDRESS`
    - 특정 리소스를 지정하는 옵션

- `-var 'NAME=VALUE'`

- `-var-file=FILENAME`


## apply


### Usage

```
$ terraform apply [options]
```


### Options

- `-auto-approve`

- `-compact-warnings`

- `-input=false`

- `-json`

- `-lock=false`

- `-lock-timeout=DURATION`

- `-no-color`

- `-parallelism=n`

- `-target=ADDRESS`
    - 특정 리소스만 지정하는 옵션


## destroy


### Usage

```
$ terraform destroy [options]
```


### Options

- `-target=ADDRESS`
    - 특정 리소스를 지정하는 옵션


## Inspecting Infrastructure


### state list


#### Usage

```
$ terraform state list [options][address...]
```


#### Options

- `-state=path`

- `-id=id`


#### Examples

- 모든 리소스 대상
```
$ terraform state list
```

- 특정 리소스 대상
```
$ terraform state list aws_instance.bar
```

- 특정 모듈 대상
```
$ terraform state list module.elb
```

- 특정 ID 대상
```
$ terraform state list -id=sg-1234qwer
```