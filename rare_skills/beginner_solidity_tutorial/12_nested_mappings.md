# Nested Mappings

In most languages, a hashmap can contain another hashmap, and Solidity does this too. However, because mappings are not valid return types, you must supply all the keys the maps require.

Here's an example:

```solidity
contract NestedMappings01 {

    mapping(uint256 => mapping(uint256 => uint256)) public nestedMap;

    function setNestedMap(
        uint256 key1,
        uint256 key2,
        uint256 finalValue
    )
        public {
            nestedMap[key1][key2] = finalValue;
    }

    function getNestedMap(
        uint256 key1,
        uint256 key2
    )
        public
        view
        returns (uint256) {
            return nestedMap[key1][key2];
    }
}
```

In another language setting the above with `1`, `2` and `100` would look like:

```javascript
{
  1: {
    2: 100
  }
}
```

Nested maps are quite common in smart contracts, unlike nested arrays.

### Public Nested Mappings Don't Work

Here's yet another strange quirk of Solidity. Solidity automatically creates getting functions for variables when you declare them as public. However, the public getter functions allow you to supply the necessary arguments.

The solution is to make nested mappings private and wrap them in a public function that gets their value.

[Original Article](https://www.rareskills.io/learn-solidity/nested-mapping)
