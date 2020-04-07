# Ethyfier

Ethyfier is a light-weight serialization format.

It's currently supporting 3 types of data :

- String
- Number
- Boolean

## Usage

```cpp
// Define the variables
Ethyfier::variable boolean{ "boolean", true };
Ethyfier::variable number{ "number", 48 };
Ethyfier::variable text{ "text", std::string("Hello, World!") };

// Serialize
std::vector<int> serialized = Ethyfier::serialize({ boolean, number, text });

// Deserialize
Ethyfier::Payload* deserialized = new Ethyfier::Payload(serialized);

// Display data
for (auto& i : deserialized->variables())
{
    std::cout << i.name << " : ";
    switch ((Ethyfier::types)i.value.index())
    {
    case Ethyfier::types::BOOL:
    	std::cout << std::get<bool>(i.value);
    	break;
    case Ethyfier::types::INT:
    	std::cout << std::get<int>(i.value);
    	break;
    case Ethyfier::types::STR:
    	std::cout << std::get<std::string>(i.value);
    	break;
    }
    std::cout << std::endl;
}
``

## Spec

Ethyfier payloads are binaries, and constructed the following way :

`variable count, [name size, type, address, name], body`

### Types

- 0 : string
- 1 : number
- 2 : boolean

### Example

Ethyfier representation of this payload : 

```json
{
	"lightweight": true,
	"version": 142,
	"description": "blablabla"
}

total: 60 bits
```

Would be :

```
Variable count  : 0x3
First variable  : 0xB, 0x2, 0x39, 0x6C, 0x69, 0x67, 0x68, 0x74, 0x77, 0x65, 0x69, 0x67, 0x68, 0x74
Second variable : 0x7, 0x1, 0x40, 0x76, 0x65, 0x72, 0x73, 0x69, 0x6F, 0x6E
Thrid variable  : 0xB, 0x0, 0x41, 0x64, 0x65, 0x73, 0x63, 0x72, 0x69, 0x70, 0x74, 0x69, 0x6F, 0x6E
Body            : 0x1, 0x142, 0x62, 0x6C, 0x61, 0x62, 0x6C, 0x61, 0x62, 0x6C, 0x61
Total           : 49 bits
```