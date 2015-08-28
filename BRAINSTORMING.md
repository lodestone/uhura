# Brainstorming A Ruby-based API to API mapper

There are three defined layers:

  - Deserializer (Format -> Native)
  - Normalizer (accounts.name -> accounts.FullName)
  - Serializer (Native -> Format)


JSONDesieralizer
Normalizer (JSONAPI -> SOAP)
XMLSerializer

## Normalizer Operations

  - Pair Key Operation
  - Pair Value Operation (coercion)
  - Add Pairs

```ruby
class Normalizer < Brainstorm::Normalizer
  normalize :request do
    pair "accounts.first_name", "accounts.name", -> { |name| name.split.first }
    pair "accounts.last_name", "accounts.name", -> { |name| name.split.last }
    pair "accounts.age", 13
  end

  def shadow.initialize(struct)
    @struct = struct
  end

  def shadow.to_request
    42
  end

  def shadow.to_response
    {
      "accounts" => {
        "first_name" => data["accounts"]["name"].split.first,
        "last_name" => data["accounts"]["name"].split.last
      }
    }
  end
end
```

## Example Input

```
POST http://api.example.com/accounts
Authentication: Bearer asdkjfnasdlkfnasdkfn
{
  "accounts": {
    "FullName": "Kurtis"
  }
}
```

```
200 OK
...
{
  "accounts": {...}
}
```


## A possible DSL?

```

define_route
  "foreign_point": "local_path",
    method: <method>,
    accepted_params: <params>

define_route
  "http://shittyAssUrl.com/api/SuperHero/SOAP.asp": "/user",
    method: :post,
    accepted_params: [:name, :age, :sex, :occupation, :secret_identity]
```
