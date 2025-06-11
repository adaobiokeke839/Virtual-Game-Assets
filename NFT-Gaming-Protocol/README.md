# GameAsset Pro - Advanced Gaming NFT Marketplace & Management System

A comprehensive SIP-009 compliant NFT implementation designed specifically for gaming ecosystems on the Stacks blockchain.

## Overview

GameAsset Pro is a sophisticated smart contract that provides a complete infrastructure for gaming NFTs, including item creation, marketplace trading, item upgrading, batch operations, and creator management. Built with security, scalability, and gaming-specific features in mind.

## Features

### Core NFT Functionality
- **SIP-009 Compliance**: Full compatibility with Stacks NFT standard
- **Gaming-Focused Asset Creation**: Rich metadata structure with traits, rarity levels, and categories
- **Batch Operations**: Execute multiple transfers in a single transaction
- **Asset Burning**: Remove assets from circulation permanently

### Marketplace System
- **Decentralized Trading**: Built-in marketplace with listing and purchasing capabilities
- **Flexible Pricing**: Support for quantity-based listings
- **Automatic Fee Collection**: Configurable marketplace fees (default 2.5%)
- **Expiration Management**: Time-based listing expiration system

### Asset Upgrade System
- **Recipe-Based Upgrades**: Define complex upgrade requirements
- **Material Consumption**: Combine multiple assets to create new ones
- **Administrative Control**: Enable/disable upgrade recipes dynamically

### Creator Management
- **Authorization System**: Whitelist approved creators
- **Creator Rights**: Original creators retain special privileges
- **Administrative Oversight**: Full administrative control over the ecosystem

## Architecture

### Data Structures

#### Game Asset Registry
Each NFT contains comprehensive metadata:
- Asset name and description
- Image URL and category
- Original creator information
- Trait system (up to 20 traits per asset)
- Extended metadata support
- Rarity level (1-10 scale)
- Tradeable status flag

#### Ownership Ledger
- Supports fractional/quantity-based ownership
- Efficient balance tracking per asset per owner
- Optimized for gaming inventory systems

#### Marketplace System
- Active listing management
- Seller indexing for efficient queries
- Automatic listing cleanup on completion

## Getting Started

### Prerequisites
- Stacks blockchain development environment
- Clarity smart contract deployment tools
- STX tokens for transaction fees

### Deployment
1. Deploy the contract to your chosen Stacks network
2. The deployer automatically becomes the contract administrator
3. Configure marketplace fee rates if needed
4. Authorize initial creators

### Basic Usage

#### Creating Assets
```clarity
;; Only authorized creators or admin can create assets
(contract-call? .gameasset-pro create-game-asset
  "Legendary Sword"           ;; asset-name
  "A powerful legendary weapon" ;; description
  "https://example.com/sword.png" ;; image-url
  "weapon"                    ;; category
  (list {trait-type: "damage", value: "150"}) ;; traits
  (some "Epic weapon with fire enchantment") ;; extended-metadata
  u9                          ;; rarity-level (1-10)
  true                        ;; is-tradeable
)
```

#### Minting Assets
```clarity
;; Mint quantity to recipient
(contract-call? .gameasset-pro mint-game-asset
  u1    ;; asset-identifier
  u5    ;; quantity-to-mint
  'SP123... ;; recipient-address
)
```

#### Marketplace Trading
```clarity
;; Create marketplace listing
(contract-call? .gameasset-pro create-marketplace-listing
  u1      ;; asset-identifier
  u1000   ;; listing-price (in microSTX)
  u3      ;; quantity-for-sale
  u12000  ;; expiration-block
)

;; Purchase from marketplace
(contract-call? .gameasset-pro purchase-from-marketplace
  u1  ;; listing-identifier
  u2  ;; purchase-quantity
)
```

## Administrative Functions

### Creator Management
```clarity
;; Authorize new creator
(contract-call? .gameasset-pro authorize-creator 'SP456...)

;; Revoke creator authorization
(contract-call? .gameasset-pro revoke-creator-authorization 'SP456...)
```

### Marketplace Configuration
```clarity
;; Update marketplace fee (in basis points, max 1000 = 10%)
(contract-call? .gameasset-pro update-marketplace-fee-rate u300) ;; 3%
```

### Transfer Ownership
```clarity
;; Transfer contract administration
(contract-call? .gameasset-pro transfer-contract-ownership 'SP789...)
```

## Asset Upgrade System

### Creating Upgrade Recipes
```clarity
;; Define upgrade recipe
(contract-call? .gameasset-pro create-upgrade-recipe
  u1  ;; base-asset-required
  (list {item-id: u2, amount: u3} {item-id: u3, amount: u1}) ;; materials
  u4  ;; resulting-asset-id
)
```

### Executing Upgrades
```clarity
;; Execute asset upgrade
(contract-call? .gameasset-pro execute-asset-upgrade u1) ;; recipe-identifier
```

## Query Functions

### Asset Information
```clarity
;; Get asset details
(contract-call? .gameasset-pro get-asset-details u1)

;; Get user balance
(contract-call? .gameasset-pro get-asset-balance u1 'SP123...)

;; Check creator authorization
(contract-call? .gameasset-pro check-creator-authorization 'SP123...)
```

### Marketplace Queries
```clarity
;; Get listing details
(contract-call? .gameasset-pro get-marketplace-listing-details u1)

;; Check if listing is active
(contract-call? .gameasset-pro check-listing-active-status u1)

;; Verify listing ownership
(contract-call? .gameasset-pro verify-user-listing_ownership 'SP123... u1)
```

## Error Codes

| Code | Constant | Description |
|------|----------|-------------|
| 100 | ERROR_UNAUTHORIZED_ACCESS | Insufficient permissions |
| 101 | ERROR_ASSET_ALREADY_EXISTS | Asset ID already in use |
| 102 | ERROR_ASSET_NOT_FOUND | Asset does not exist |
| 103 | ERROR_INSUFFICIENT_QUANTITY | Not enough assets owned |
| 104 | ERROR_TRANSFER_OPERATION_FAILED | Transfer failed |
| 105 | ERROR_MARKETPLACE_LISTING_NOT_FOUND | Listing not found |
| 106 | ERROR_MARKETPLACE_LISTING_EXPIRED | Listing has expired |
| 107 | ERROR_INVALID_PRICE_VALUE | Invalid price specified |
| 108 | ERROR_SELF_TRANSFER_ATTEMPT | Cannot transfer to self |
| 109 | ERROR_INVALID_PRINCIPAL_ADDRESS | Invalid address |
| 110 | ERROR_INVALID_INPUT_PARAMETER | Invalid parameter |
| 111 | ERROR_EMPTY_STRING_PROVIDED | Empty string not allowed |
| 112 | ERROR_INVALID_ASSET_ATTRIBUTES | Invalid asset traits |

## Security Features

- **Input Validation**: Comprehensive validation of all inputs
- **Access Control**: Multi-level authorization system
- **Principal Validation**: Protection against invalid addresses
- **Overflow Protection**: Safe arithmetic operations
- **State Consistency**: Atomic operations with rollback on failure

## Use Cases

### Gaming Applications
- **RPG Items**: Weapons, armor, consumables with complex trait systems
- **Trading Card Games**: Cards with rarity and gameplay attributes
- **Virtual Real Estate**: Land parcels with location-based metadata
- **Character Assets**: Player avatars and customization items

### Marketplace Integration
- **In-Game Stores**: Native marketplace integration
- **Cross-Game Trading**: Assets usable across multiple games
- **Guild Systems**: Batch transfers for guild management
- **Tournament Rewards**: Automated prize distribution

## Scalability Considerations

- **Efficient Storage**: Optimized data structures for gas efficiency
- **Batch Operations**: Reduce transaction costs with bulk operations
- **Indexed Queries**: Fast lookups for marketplace and ownership data
- **Modular Design**: Easy integration with external systems

## Contributing

This smart contract is designed to be extended and customized for specific gaming needs. Key extension points include:

- Custom trait validation logic
- Additional marketplace features (auctions, offers)
- Enhanced upgrade system mechanics
- Integration with external oracle systems