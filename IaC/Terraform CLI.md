# Terraform CLI Commands


## Initializing Working Directories


### `init`


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

- `-reconfigure`
    - This option disregards any existing configuration, preventing migration of any existing state


#### Examples

- Initialize the current working directory
```
$ terraform init
```

- Reconfigure
```
$ terraform init -reconfigure
```


## Provisioning Infrastructure


### `plan`


#### Usage

```
$ terraform plan [options]
```


#### Options

- `-refresh=false`

- `-replace=ADDRESS`

- `-target=ADDRESS`
    - Filtering by Resource

- `-var 'NAME=VALUE'`

- `-var-file=FILENAME`


#### Examples

- s
```
```


### `apply`


#### Usage

```
$ terraform apply [options]
```


#### Options

- `-auto-approve`

- `-compact-warnings`

- `-input=false`

- `-json`

- `-lock=false`

- `-lock-timeout=DURATION`

- `-no-color`

- `-parallelism=n`

- `-target=ADDRESS`
    - Filtering by Resource


#### Examples

- f
```
```


### `destroy`


#### Usage

```
$ terraform destroy [options]
```


#### Options

- `-target=ADDRESS`
    - Filtering by Resource


#### Examples

- d
```
```


## Inspecting Infrastructure


### `state list`


#### Usage

```
$ terraform state list [options][address...]
```


#### Options

- `-state=path`
    - Path to the state file
    - Defaults to `terraform.tfstate`
    - Ignored when `remote state` is used

- `-id=id`
    - ID of resources to show
    - Ignored when unset


#### Examples

- All Resources
```
$ terraform state list
```

- Filtering by Resource
```
$ terraform state list aws_instance.bar
```

- Filtering by Module
```
$ terraform state list module.elb
```

- Filtering by ID
```
$ terraform state list -id=sg-1234qwer
```