![](https://github.com/jeyakatsa/Jehova/blob/main/assets/Jehova-Logo.jpg)

# Jehova: A Smart-Contract Language for Java Developers

## *The Proposal*

### Abstract
Siphoning advice aggregated from a few core developers from within the Ethereum foundation, the best path to traverse in order for Java-like fundamentals to be abstracted and compiled into the EVM, is for a brand-new programming language to be erected capable of running Java-like programs on the EVM: *Jehova*.

### Motivation
Currently, there are 200 thousand Solidity/Ethereum Developers and 7 million Java Developers Worldwide respectfully. Thus, allowing Smart-Contracts to be built in a language familiar with Java developers would help onboard more developers into the Ethereum Ecosystem.

### Build Tools
The main goal is for Smart-Contracts on Ethereum to be built with Java tools like Gradle as to remain relevant with Java clients like Consensys with plans to expand to build tools like Maven and Jenkins in order to remain independent from any client in the future.

### Language Examples

#### Smart-Contract Storage example in *Solidity*
```solidity
pragma solidity >=0.4.16 <0.9.0;

contract SimpleStorage {
    uint storedData;

    function set(uint x) public {
        storedData = x;
    }

    function get() public view returns (uint) {
        return storedData;
    }
}
```

#### Smart-Contract Storage example in *Jehova*
```java
public class SimpleStorage {
    private Uint256 storedData;

    public void setStoredData (Uint256 storedData) {
        this.storedData = storedData;
    }

    public Uint256 getStoredData () {
        return storedData;
    }
}
```


*Note: This R&D project is still early in its development, so questions/contributions/conversations are heavily welcome.*


## Early Work
- [Java Smart Contract Abstraction for Ethereum R&D](https://github.com/jeyakatsa/ethereum-smart-contract-java-abstraction/tree/main/R%26D-files)
- [Java Smart Contract Abstraction for Ethereum](https://github.com/jeyakatsa/Ethereum-Smart-Contract-Java-Abstraction)
