# -Supply-Chain-Transparency-Smart-Contract-for-EVM-networks

Building a simple Supply Chain Transparency Smart Contract for any EVM network, focusing on creating a blockchain-based provenance tracking system that enhances transparency and prevents fraud in supply chains.

# Context
Businesses and consumers need to verify the authenticity of products, ensuring they are not counterfeit or altered. Blockchain provides an immutable record of a product’s journey from manufacturer to retailer.

# Provenance Tracking System procedure: 

* Manufacturers Register Products: each product gets a unique ID on the blockchain.
  
* Logistics Updates: Distributors update the status and location at each step.
  
* Retailers Verify Authenticity: Customers scan a QR code linked to blockchain records.
  
* Immutable Audit Trail: all transactions remain visible and immutable.


# 1). Define the Product Structure 

``` 
pragma solidity ^0.8.0;

contract SupplyChain {
    struct Product {
        string name;
        string manufacturer;
        uint timestamp;
        address currentHolder;
        string status;
    }

    mapping(uint => Product) public products;
    uint public productCount;

    event ProductAdded(uint productId, string name, string manufacturer, address indexed owner);
    event StatusUpdated(uint productId, string newStatus, address indexed updatedBy);

    function addProduct(string memory _name, string memory _manufacturer) public {
        productCount++;
        products[productCount] = Product(_name, _manufacturer, block.timestamp, msg.sender, "Manufactured");

        emit ProductAdded(productCount, _name, _manufacturer, msg.sender);
    }

   ```

#  2) Update Product Status

```
    function updateStatus(uint _productId, string memory _newStatus) public {
        require(products[_productId].currentHolder == msg.sender, "Not authorized to update status");
        products[_productId].status = _newStatus;

        emit StatusUpdated(_productId, _newStatus, msg.sender);
    }
}



```

#  3) Frontend Integration

* Create a Button to Add a Product: Calls ```addProduct(name, manufacturer) ```
* Create a Button to Update Product Status: Calls ```updateStatus(productId, newStatus) ```.
* Add a QR Code Scanner: Retrieves product ID and displays blockchain data.
