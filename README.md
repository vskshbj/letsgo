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
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Rating {
    uint8 public rating;

    function setRating(uint8 _rating) external {
        require(_rating >= 1 && _rating <= 5, "Invalid rating");
        rating = _rating;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PasswordChecker {
    string private password = "base";

    function check(string calldata attempt) external view returns (bool) {
        return keccak256(abi.encodePacked(attempt)) == keccak256(abi.encodePacked(password));
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MathHelper {
    uint256 public value;

    function add(uint256 amount) external {
        value += amount;
    }

    function subtract(uint256 amount) external {
        value -= amount;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract DayCounter {
    uint256 public calls;

    function hit() external {
        calls++;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Percentage {
    function calc(uint256 amount, uint256 percent) external pure returns (uint256) {
        return (amount * percent) / 100;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Doubler {
    function double(uint256 number) external pure returns (uint256) {
        return number * 2;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Square {
    function calc(uint256 number) external pure returns (uint256) {
        return number * number;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Triple {
    function calc(uint256 number) external pure returns (uint256) {
        return number * 3;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PowerOfTwo {
    function calc(uint256 exp) external pure returns (uint256) {
        return 2 ** exp;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Remainder {
    function calc(uint256 a, uint256 b) external pure returns (uint256) {
        require(b > 0, "Division by zero");
        return a % b;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MaxOfTwo {
    function max(uint256 a, uint256 b) external pure returns (uint256) {
        return a > b ? a : b;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Absolute {
    function abs(int256 number) external pure returns (uint256) {
        return number >= 0 ? uint256(number) : uint256(-number);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract EqualChecker {
    function isEqual(uint256 a, uint256 b) external pure returns (bool) {
        return a == b;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract NotEqual {
    function isDifferent(uint256 a, uint256 b) external pure returns (bool) {
        return a != b;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract GreaterOrEqual {
    function check(uint256 a, uint256 b) external pure returns (bool) {
        return a >= b;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SumThree {
    function sum(uint256 a, uint256 b, uint256 c) external pure returns (uint256) {
        return a + b + c;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Product {
    function multiply(uint256 a, uint256 b) external pure returns (uint256) {
        return a * b;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Division {
    function divide(uint256 a, uint256 b) external pure returns (uint256) {
        require(b > 0, "Cannot divide by zero");
        return a / b;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract IsEven {
    function check(uint256 number) external pure returns (bool) {
        return number % 2 == 0;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract IsOdd {
    function check(uint256 number) external pure returns (bool) {
        return number % 2 != 0;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PercentOf100 {
    function calc(uint256 value) external pure returns (uint256) {
        return (value * 100) / 100; // simplified example
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ModuloTen {
    function calc(uint256 number) external pure returns (uint256) {
        return number % 10;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract IsPositive {
    function check(uint256 number) external pure returns (bool) {
        return number > 0;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Clamp {
    function clamp(uint256 value, uint256 min, uint256 max) external pure returns (uint256) {
        if (value < min) return min;
        if (value > max) return max;
        return value;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SafeAdd {
    function add(uint256 a, uint256 b) external pure returns (uint256) {
        return a + b;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Difference {
    function diff(uint256 a, uint256 b) external pure returns (uint256) {
        return a > b ? a - b : b - a;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract RangeChecker {
    function inRange(uint256 value, uint256 min, uint256 max) external pure returns (bool) {
        return value >= min && value <= max;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Power {
    function calc(uint256 base, uint256 exp) external pure returns (uint256) {
        return base ** exp;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Sqrt {
    function sqrt(uint256 x) external pure returns (uint256) {
        if (x == 0) return 0;
        uint256 z = (x + 1) / 2;
        uint256 y = x;
        while (z < y) {
            y = z;
            z = (x / z + z) / 2;
        }
        return y;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract BitwiseAnd {
    function and(uint256 a, uint256 b) external pure returns (uint256) {
        return a & b;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract RightShift {
    function shift(uint256 value, uint256 positions) external pure returns (uint256) {
        return value >> positions;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract AddressToUint {
    function convert(address addr) external pure returns (uint256) {
        return uint256(uint160(addr));
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract EncodePacked {
    function encode(uint256 a, address b) external pure returns (bytes memory) {
        return abi.encodePacked(a, b);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract RequireExample {
    function check(uint256 value) external pure {
        require(value > 0, "Value must be positive");
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ModifierExample {
    address public owner = msg.sender;

    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }

    function restricted() external onlyOwner {}
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract StructExample {
    struct Person {
        string name;
        uint256 age;
    }

    Person public person;

    function set(string calldata _name, uint256 _age) external {
        person = Person(_name, _age);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract NestedMapping {
    mapping(address => mapping(address => uint256)) public allowances;

    function set(address spender, uint256 amount) external {
        allowances[msg.sender][spender] = amount;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Parent {
    uint256 public value;
}

contract Child is Parent {
    function setValue(uint256 _value) external {
        value = _value;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract TryCatchExample {
    function tryCall(address target) external returns (bool success) {
        try this.externalFunction() {
            success = true;
        } catch {
            success = false;
        }
    }

    function externalFunction() external pure {}
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ImmutableOwner {
    address public immutable owner;

    constructor() {
        owner = msg.sender;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract UncheckedExample {
    function add(uint256 a, uint256 b) external pure returns (uint256) {
        unchecked {
            return a + b;
        }
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract TransientDemo {
    function setTransient(uint256 value) external {
        assembly {
            tstore(0, value)
        }
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract BaseFee {
    function getBaseFee() external view returns (uint256) {
        return block.basefee;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MsgDataLength {
    function getLength() external pure returns (uint256) {
        return msg.data.length;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Create2Helper {
    function computeAddress(bytes32 salt, bytes32 bytecodeHash) external view returns (address) {
        return address(uint160(uint256(keccak256(abi.encodePacked(bytes1(0xff), address(this), salt, bytecodeHash)))));
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract StaticcallExample {
    function staticCall(address target) external view returns (bool) {
        (bool success, ) = target.staticcall("");
        return success;
    }
}// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract BytesSlice {
    function slice(bytes calldata data, uint256 start, uint256 end) external pure returns (bytes memory) {
        return data[start:end];
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MappingCounter {
    mapping(address => uint256) public values;
    uint256 public count;

    function set(uint256 value) external {
        if (values[msg.sender] == 0 && value > 0) count++;
        values[msg.sender] = value;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract FallbackOnly {
    fallback() external {}
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract IfElse {
    function check(uint256 value) external pure returns (string memory) {
        if (value > 10) {
            return "High";
        } else {
            return "Low";
        }
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract DoWhile {
    function run(uint256 n) external pure returns (uint256) {
        uint256 i = 0;
        do {
            i++;
        } while (i < n);
        return i;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SwitchLike {
    function getDay(uint256 day) external pure returns (string memory) {
        if (day == 1) return "Monday";
        if (day == 2) return "Tuesday";
        if (day == 3) return "Wednesday";
        return "Other";
    }
}// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SwitchLike {
    function getDay(uint256 day) external pure returns (string memory) {
        if (day == 1) return "Monday";
        if (day == 2) return "Tuesday";
        if (day == 3) return "Wednesday";
        return "Other";
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Overloading {
    function add(uint256 a, uint256 b) external pure returns (uint256) {
        return a + b;
    }

    function add(uint256 a, uint256 b, uint256 c) external pure returns (uint256) {
        return a + b + c;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ModifierArg {
    modifier greaterThan(uint256 value, uint256 min) {
        require(value > min, "Too small");
        _;
    }

    function set(uint256 value) external greaterThan(value, 10) {}
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ParentOverride {
    function getValue() public pure virtual returns (uint256) {
        return 1;
    }
}

contract ChildOverride is ParentOverride {
    function getValue() public pure override returns (uint256) {
        return 2;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

interface IBase {
    function base() external pure returns (uint256);
}

interface IExtended is IBase {
    function extended() external pure returns (uint256);
}

contract Implementation is IExtended {
    function base() external pure returns (uint256) { return 1; }
    function extended() external pure returns (uint256) { return 2; }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract AnonymousEvent {
    event Log(uint256 value) anonymous;

    function emitLog(uint256 value) external {
        emit Log(value);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ErrorSelector {
    error MyError();

    function getSelector() external pure returns (bytes4) {
        return MyError.selector;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract StoragePacking {
    uint128 public a;
    uint128 public b; // packed in same slot
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SstoreExample {
    function write(uint256 value) external {
        assembly {
            sstore(0, value)
        }
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract KeccakAssembly {
    function hash(uint256 value) external pure returns (bytes32 result) {
        assembly {
            mstore(0x00, value)
            result := keccak256(0x00, 0x20)
        }
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ChainidAssembly {
    function getChainId() external view returns (uint256 id) {
        assembly {
            id := chainid()
        }
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract BytesToHex {
    function toHex(bytes calldata data) external pure returns (string memory) {
        bytes memory hexChars = "0123456789abcdef";
        bytes memory result = new bytes(data.length * 2);
        for (uint256 i = 0; i < data.length; i++) {
            result[i * 2] = hexChars[uint8(data[i] >> 4)];
            result[i * 2 + 1] = hexChars[uint8(data[i] & 0x0f)];
        }
        return string(result);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ArrayContains {
    function contains(uint256[] calldata arr, uint256 value) external pure returns (bool) {
        for (uint256 i = 0; i < arr.length; i++) {
            if (arr[i] == value) return true;
        }
        return false;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ArrayReverse {
    function reverse(uint256[] memory arr) external pure returns (uint256[] memory) {
        uint256 n = arr.length;
        for (uint256 i = 0; i < n / 2; i++) {
            (arr[i], arr[n - 1 - i]) = (arr[n - 1 - i], arr[i]);
        }
        return arr;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract NestedStruct {
    struct Info {
        string name;
        uint256 age;
    }

    struct User {
        address addr;
        Info info;
    }

    User public user;

    function set(address addr, string calldata name, uint256 age) external {
        user = User(addr, Info(name, age));
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract BytesConversion {
    function toBytes32(bytes1 b) external pure returns (bytes32) {
        return bytes32(b);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract GasExample {
    uint256 public value;

    function set(uint256 _value) external {
        value = _value;
    }

    function reset() external {
        value = 0; // can help with gas refunds in some cases
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ReentrancyGuard {
    bool private locked;

    modifier nonReentrant() {
        require(!locked, "Reentrant");
        locked = true;
        _;
        locked = false;
    }

    function protected() external nonReentrant {}
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract RoleAccess {
    mapping(address => bool) public admins;

    constructor() {
        admins[msg.sender] = true;
    }

    modifier onlyAdmin() {
        require(admins[msg.sender], "Not admin");
        _;
    }

    function addAdmin(address user) external onlyAdmin {
        admins[user] = true;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract RateLimiter {
    mapping(address => uint256) public lastCall;

    function call() external {
        require(block.timestamp >= lastCall[msg.sender] + 1 minutes, "Too soon");
        lastCall[msg.sender] = block.timestamp;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract VotingPower {
    mapping(address => uint256) public power;

    function setPower(uint256 amount) external {
        power[msg.sender] = amount;
    }
}// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract BalanceSnapshot {
    mapping(address => uint256) public snapshots;

    function snapshot(uint256 balance) external {
        snapshots[msg.sender] = balance;
    }
}
