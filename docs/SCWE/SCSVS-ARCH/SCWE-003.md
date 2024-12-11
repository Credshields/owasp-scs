---
title: Lack of Modularity
id: SCWE-003
alias: lack-of-modularity
platform: []
profiles: [L1]
mappings:
  scsvs-cg: [SCSVS-ARCH]
  scsvs-scg: [SCSVS-ARCH-1]
  cwe: [1047]
status: new
---

## Relationships
- CWE-1047: Modules with Circular Dependencies
  [https://cwe.mitre.org/data/definitions/1047.html](https://cwe.mitre.org/data/definitions/1047.html)

## Description
Lack of modularity refers to a design flaw where a system's components are not sufficiently separated into independent, reusable modules. This deficiency leads to tightly coupled code, making the system difficult to understand, maintain, and extend. In the context of smart contracts, this manifests as monolithic contracts where all functionalities are bundled together, increasing complexity and the potential for errors.

## Remediation
- **Modular Design:** Break down the contract into smaller, focused modules that handle specific responsibilities.
- **Use of Libraries:** Leverage existing, well-tested libraries to handle common functionalities, reducing the need for custom code.
- **Simplify Logic:** Avoid unnecessary complexity by streamlining the contract's logic and removing redundant code.
- **Regular Audits:** Conduct periodic code reviews and audits to identify and address areas of excessive complexity.

## Samples

### Example of Lack of Modularity:

```solidity
pragma solidity ^0.4.0;

contract MonolithicContract {
    uint public balance;
    address public owner;
    mapping(address => uint) public allowances;

    function deposit(uint value) public {
        balance += value;
    }

    function withdraw(uint value) public {
        require(balance >= value, "Insufficient funds");
        balance -= value;
    }

    function transfer(address to, uint value) public {
        require(balance >= value, "Insufficient funds");
        balance -= value;
        to.transfer(value);
    }

    function approve(address spender, uint value) public {
        allowances[spender] = value;
    }

    function transferFrom(address from, address to, uint value) public {
        require(allowances[from] >= value, "Allowance exceeded");
        allowances[from] -= value;
        to.transfer(value);
    }

    function changeOwner(address newOwner) public {
        owner = newOwner;
    }
}
```

### Refactored with Modular Design:

```solidity
pragma solidity ^0.4.0;

contract Balance {
    uint public balance;

    function deposit(uint value) public {
        balance += value;
    }

    function withdraw(uint value) public {
        require(balance >= value, "Insufficient funds");
        balance -= value;
    }
}

contract Transfer {
    uint public balance;

    function transfer(address to, uint value) public {
        require(balance >= value, "Insufficient funds");
        balance -= value;
        to.transfer(value);
    }
}

contract Allowance {
    mapping(address => uint) public allowances;

    function approve(address spender, uint value) public {
        allowances[spender] = value;
    }

    function transferFrom(address from, address to, uint value) public {
        require(allowances[from] >= value, "Allowance exceeded");
        allowances[from] -= value;
        to.transfer(value);
    }
}

contract Ownership {
    address public owner;

    function changeOwner(address newOwner) public {
        owner = newOwner;
    }
}
```
