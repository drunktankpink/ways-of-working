# Variables & Outputs

## Variables

1. All [input variables](https://www.terraform.io/language/values/variables) must contain `type` and `description` arguments
   
2. To ensure a variable is required, omit the `default` argument from the declaration. 
    >**Note:** It may also be worth considering the [disallowing](https://www.terraform.io/language/values/variables#disallowing-null-input-values) of `null` make sense.

3. Order arguments in a variable block like this: `description` , `type`, `default`, `validation`.

4. Prefer using simple types (number, string, `list(...)`, `map(...)`, `any`) over specific type like `object()` unless you need to have strict constraints on each key.

5. Use specific types like `map(map(string))` if all elements of the map have the same type (e.g. `string`) or can be converted to it (e.g. `number` type can be converted to `string`)

6. Use the plural form in a variable name when type is `list(...)` or `map(...)`.

7. Where input is subjective, or where specific behaviour needs to be prevented, variable [validation](https://www.terraform.io/language/values/variables#custom-validation-rules) should be used. 
    > **Note:** more variable validation examples can be found [here](https://dev.to/drewmullen/terraform-variable-validation-with-samples-1ank)

## Outputs

1. All [output values](https://www.terraform.io/language/values/outputs) should consistent and understandable outside its scope. It should be obvious what type and attribute is returned.

2. The name of the output should describe the property it contains.

3. Use the plural form in a output name when type is `list(...)`.

4. A good structure for the output name is `<name>_<type>_<attribute>`, where:
   1. `<name>` is the resource or data source name without the provider prefix
      - `aws_subnet` is `subnet` or `aws_vpc` is `vpc`
   2. `<type>` is a type of resource sources
   3. `<attribute>` is the attribute return by the output

5. Always include a `description` for all outputs, even if you think its obvious.

6. Prefer try() (available since Terraform 0.13) over element(concat(...)) (legacy approach for the version before 0.13)