Base is an optimistic rollup built on the OP Stack that aims to make onchain apps more accessible, cheaper and developer-friendly. This repo kicks off my journey contributing to the Base ecosystem.
docs: add project overview and Base network details

Base is an OP Stack L2 secured by Ethereum. This update documents the chain ID (8453), RPC endpoints and the main goals of this repository: learning and shipping onchain apps.
feat: add SimpleStorage contract for Base

Created a basic SimpleStorage.sol contract with set and get functions. First smart contract ready to deploy on Base Sepolia.
feat: add owner-only modifier to SimpleStorage

Introduced an onlyOwner modifier and restricted the set function. Improves security for contracts deployed on Base.
feat: add getOwner function to SimpleStorage

Exposed a public getOwner view function. Makes it easier to verify ownership after deploying on Base.
feat: add pause functionality to SimpleStorage

Implemented a pause mechanism so the owner can temporarily disable the set function. Useful safety feature for contracts on Base.
feat: add version tracking to SimpleStorage

Introduced a version variable that increments on important changes. Helps track contract upgrades and history on Base.
feat: add batch set function to SimpleStorage

Implemented a function that allows setting multiple values in a single transaction. Reduces gas costs when interacting on Base.
feat: add data validation to SimpleStorage

Added checks to prevent storing empty or invalid values. Makes the contract more reliable when used on Base.
feat: add timestamp tracking to SimpleStorage

Every time a value is stored, the contract now also records the block timestamp. Useful for time-based logic on Base.
feat: add retrieve all data function to SimpleStorage

Created a view function that returns both the stored value and its timestamp in one call. Cleaner interaction on Base.
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SimpleStorage {
    address public owner;
    uint256 private storedValue;
    uint256 public lastUpdated;
    bool public paused;

    event ValueStored(address indexed user, uint256 value, uint256 timestamp);
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    event Paused(address account);
    event Unpaused(address account);

    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }

    modifier whenNotPaused() {
        require(!paused, "Paused");
        _;
    }

    constructor() {
        owner = msg.sender;
    }

    function set(uint256 value) external onlyOwner whenNotPaused {
        storedValue = value;
        lastUpdated = block.timestamp;
        emit ValueStored(msg.sender, value, block.timestamp);
    }

    function get() external view returns (uint256, uint256) {
        return (storedValue, lastUpdated);
    }

    function pause() external onlyOwner {
        paused = true;
        emit Paused(msg.sender);
    }

    function unpause() external onlyOwner {
        paused = false;
        emit Unpaused(msg.sender);
    }

    function transferOwnership(address newOwner) external onlyOwner {
        require(newOwner != address(0), "Zero address");
        emit OwnershipTransferred(owner, newOwner);
        owner = newOwner;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract OwnableStorage {
    address public owner;
    uint256 public value;

    event ValueChanged(uint256 newValue);

    constructor() {
        owner = msg.sender;
    }

    function setValue(uint256 _value) external {
        require(msg.sender == owner, "Not owner");
        value = _value;
        emit ValueChanged(_value);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract NameRegistry {
    mapping(address => string) public names;

    function setName(string calldata _name) external {
        names[msg.sender] = _name;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MessageBoard {
    string public lastMessage;
    address public lastSender;

    function post(string calldata _message) external {
        lastMessage = _message;
        lastSender = msg.sender;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract BalanceTracker {
    uint256 public lastAmount;

    receive() external payable {
        lastAmount = msg.value;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Scoreboard {
    uint256 public highScore;

    function submitScore(uint256 score) external {
        require(score > highScore, "Not high enough");
        highScore = score;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract FavoriteNumber {
    mapping(address => uint256) public favorites;

    function setFavorite(uint256 number) external {
        favorites[msg.sender] = number;
    }
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ColorPicker {
    string public favoriteColor = "blue";

    function setColor(string calldata color) external {
        favoriteColor = color;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Temperature {
    int256 public celsius;

    function setTemperature(int256 _celsius) external {
        celsius = _celsius;
    }
}
