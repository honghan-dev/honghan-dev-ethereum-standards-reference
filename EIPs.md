## Common EIP/ERC Standards

### Token Standard

- [ERC20](#erc20) Fungible Token Standard
- [ERC-721](#erc721) Non-Fungible Token Standard
- [ERC-1155](#erc1155) Multi-Token Standard
- [ERC-6909](#erc6909) Minimal Multi-Token Interface
- [ERC-4626](#erc4626) Tokenized Vault Standard

### Signature Standard

- [EIP-191](#erc191) Signature Standard
- [EIP-712](#erc712) Typed structured data hashing and signing

### Proxy Standard

- [EIP-1967](#eip1967) Proxy Standard
- [EIP-1822](#eip1822) Universal Upgradable Proxy Standard

<a id="erc20"></a>

## ERC-20: Fungible Token Standard

<details>

<summary> Key Functions: </summary>

```javascript
function balanceOf(address account) public view returns (uint256)
```

- Returns the token balance of the given account. Essential for checking token holdings of any address.

```javascript
function transfer(address recipient, uint256 amount) public returns (bool)
```

- Transfers amount of tokens from the caller's account to recipient. Emits a Transfer event. Returns true if the transfer was successful.

```javascript
function approve(address spender, uint256 amount) public returns (bool)
```

- Allows spender to withdraw from the caller's account up to the amount. Emits an Approval event. Used to enable transfers on behalf of the token owner.

```javascript
function transferFrom(address sender, address recipient, uint256 amount) public returns (bool)
```

- Moves amount tokens from sender to recipient using the allowance mechanism. Amount is deducted from the caller's allowance. Emits a Transfer event.

```javascript
function allowance(address owner, address spender) public view returns (uint256)
```

- Returns the remaining number of tokens that spender is allowed to spend on behalf of owner.

</details>

<a id="erc721"></a>

## ERC-721: Non-fungible tokens (NFTs)

<details>

<summary> Key Functions: </summary>

```javascript
function balanceOf(address owner) public view returns (uint256)

```

- Returns the number of NFTs owned by owner. Used to query the NFT holdings of an address.

```javascript
function ownerOf(uint256 tokenId) public view returns (address)
```

- Returns the owner of the NFT with the given tokenId. Essential for verifying ownership of a specific NFT.

```javascript
function safeTransferFrom(address from, address to, uint256 tokenId, bytes memory data) public
```

- Safely transfers the ownership of a given NFT tokenId from from to to. Checks if to is a smart contract, and if so, calls onERC721Received.

```javascript
function approve(address to, uint256 tokenId) public
```

- Grants permission to to to transfer the NFT with tokenId. Emits an Approval event.

```javascript
function getApproved(uint256 tokenId) public view returns (address)
```

- Returns the address approved for tokenId. Used to check who has permission to transfer a specific NFT.

</details>

<a id="erc1155"></a>

## ERC-1155: Multi-Token Standard

### Description

ERC-1155 allows for managing multiple token types (both fungible and non-fungible) within a single contract.

Uses `uint256 id` to represent different token types.

<details>

<summary> Key Functions: </summary>

```javascript
function balanceOf(address account, uint256 id) public view returns (uint256)
```

- Returns the balance of token type id owned by account.

```javascript
function balanceOfBatch(address[] memory accounts, uint256[] memory ids) public view returns (uint256[] memory)
```

- Returns the balance of multiple account/token pairs. Useful for batch balance checking.

```javascript
function safeTransferFrom(address from, address to, uint256 id, uint256 amount, bytes memory data) public
```

- Transfers amount of token type id from from to to. Safer than transferFrom as it checks if to is a contract.

```javascript
function safeBatchTransferFrom(address from, address to, uint256[] memory ids, uint256[] memory amounts, bytes memory data) public
```

- Batch-transfers multiple token types at once. Efficient for transferring multiple tokens in a single transaction.

```javascript
function setApprovalForAll(address operator, bool approved) public
```

Enables or disables approval for a third party ("operator") to manage all of the caller's tokens.

</details>

<a id="erc6909"></a>

## ERC-6909: Minimal Multi-Token Interface

### Description

- Uses `uint256 id` to represent different token types.
- Similar to [ERC1155](#erc1155) but with a simpler interface.

<details>

<summary>Key functions</summary>

```javascript
// uint256 id is used to represent different token types
function balanceOf(address owner, uint256 id) public view returns (uint256);
function transfer(address receiver, uint256 id, uint256 amount) public returns (bool);
function transferFrom(address sender, address receiver, uint256 id, uint256 amount) public returns (bool);
```

</details>

<a id="erc4626"></a>

## ERC-4626: Tokenized Vault Standard

### Description

- Standard for tokenized vaults, providing a standard API for yield-bearing vaults that are represented by ERC-20 tokens
- Inherits from [ERC-20](#erc20) standard

<details>

<summary> Key Functions: </summary>

```javascript
function deposit(uint256 assets, address receiver) public virtual returns (uint256)
function mint(uint256 shares, address receiver) public virtual returns (uint256) 
function withdraw(uint256 assets, address receiver, address owner) public virtual returns (uint256)s

// From assets to shares, with support for rounding direction.
function _convertToAssets(uint256 shares, Math.Rounding rounding) internal view virtual returns (uint256)

// From shares to asset, with support for rounding direction.
function _convertToShares(uint256 assets, Math.Rounding rounding) internal view virtual returns (uint256) 
```

</details>

<a id="erc191"></a>

## ERC-191: Signature Standard

- Standardizing off-chain message signing and on-chain message recovery
- `0x19 <0x45 (E)> <thereum Signed Message:\n" + len(message)> <data to sign>`

<details>

<summary> Key concept: </summary>

1. Purpose: Distinguishes Ethereum signed messages from other signed data.

2. Structure:

a. `0x19`: Magic byte to prevent collision with EIP-155 signatures

b. `<version byte>`: Indicates the type of signature

- `0x45 ("E")`: Personal signature
- `0x00`: Data with "intended validator"
- `0x01`: Structured data (used by EIP-712)

`<version specific data>`: Depends on the version byte

3. Benefits:

Prevents signed messages from being mistaken for transactions. Allows for different signing schemes (personal, structured, etc.) .Enhances security in cross-platform and cross-contract scenarios

4. Usage:

- Off-chain: Used to create standardized signatures
- On-chain: Used to verify signatures and recover signer addresses

5. Implementation:

- Off-chain: Typically handled by wallet software or libraries
- On-chain: Verification using ecrecover in smart contracts

6. Compatibility:

Widely supported by Ethereum wallets and libraries
Forms the basis for more complex standards like [EIP-712](#erc712)

</details>

<a id="erc712"></a>

## ERC-712: Typed structured data hashing and signing

**EIP712 was designed to address these two problems:**

- It introduces the use of DOMAIN_SEPARATOR to prevent replay attacks.
- It provides a standard for encoding struct data.

```javascript
// 0x19 <1 byte version> <version specific data> <data to sign>

0x19 0x01 DOMAIN_SEPARATOR_HASH messageHash
```

```javascript
// DOMAIN_SEPARATOR
struct EIP712Domain {
    string name;
    string version;
    uint256 chainId;
    address verifyingContract;
    bytes32 salt;  // optional
}

// Call message
struct ExampleMessage {
    address from;
    address to;
    string content;
}
```

**Which Fields Need to be Hashed when constructing `DOMAIN_SEPARATOR`?**

Always Hash:

- `string`
- `bytes`
- Dynamic arrays (e.g., `uint256[]`, `bytes[]`)

Never Hash:

- `uint256` (including uint8, uint16, etc.)
- `int256` (including int8, int16, etc.)
- `bool`
- `address`
- `Fixed-size arrays` (e.g., bytes32, uint256[3])

<a id="eip1967"></a>

## EIP-1967: Standard Proxy Storage Slots

**Purpose**
  
  - Standardizes storage slots for any proxy pattern contracts to avoid clashes and enhance compatibility.

<details>

<summary>Key Features</summary>

1. Standardized Storage Slots:

   - Implementation: `0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc`
   - Admin: `0xb53127684a568b3173ae13b9f8a6016e243e63b6e8ee1178d6a717850b5d6103`
   - Beacon: `0xa3f0ad74e5423aebfd80d3ef4346578335a9a72aeaee59ff6cb3582b35133d50`

2. Unstructured Storage Pattern:

   - Uses specific storage slots to prevent collisions with implementation contract storage.

3. Enhanced Compatibility:

   - Allows different proxy implementations to be compatible with each other.

**Implementation**

```javascript
contract EIP1967Proxy {
    bytes32 private constant IMPLEMENTATION_SLOT = 0x360894...;

    function _setImplementation(address newImplementation) private {
        assembly {
            sstore(IMPLEMENTATION_SLOT, newImplementation)
        }
    }

    function _implementation() internal view returns (address impl) {
        assembly {
            impl := sload(IMPLEMENTATION_SLOT)
        }
    }
}
```

**Use Cases**

- Creating upgradeable contracts with standardized storage layouts.
- Ensuring compatibility between different proxy implementations.

</details>