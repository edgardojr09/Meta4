// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract DegenToken is ERC20, Ownable {
    event TicketCreated(address indexed creator, string eventName, uint256 quantity, uint256 price);
    event TicketRedeemed(address indexed redeemer, string eventName, uint256 quantity);
    event TicketTransferred(address indexed from, address indexed to, string eventName, uint256 quantity);

    struct Ticket {
        uint256 quantity;
        uint256 price;
    }

    mapping(string => Ticket) public tickets;
    mapping(address => mapping(string => uint256)) public userTickets;

    constructor(address initialOwner) Ownable(initialOwner) ERC20("Degen", "DGN") {}

    function mint(address to, uint256 amount) external onlyOwner {
        _mint(to, amount);
    }

    function burn(uint256 amount) external {
        _burn(msg.sender, amount);
    }

    function transfer(address to, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), to, amount);
        return true;
    }

    function createTicket(string memory eventName, uint256 quantity, uint256 price) external onlyOwner {
        require(quantity > 0, "Quantity must be greater than zero");
        require(price > 0, "Price must be greater than zero");

        tickets[eventName] = Ticket({
            quantity: quantity,
            price: price
        });

        emit TicketCreated(msg.sender, eventName, quantity, price);
    }

    function buyTicket(string memory eventName, uint256 quantity) external {
        Ticket storage ticket = tickets[eventName];
        require(ticket.quantity >= quantity, "Not enough tickets available");
        uint256 totalPrice = ticket.price * quantity;
        require(balanceOf(msg.sender) >= totalPrice, "Insufficient balance");

        _burn(msg.sender, totalPrice);
        ticket.quantity -= quantity;
        userTickets[msg.sender][eventName] += quantity;

        emit TicketRedeemed(msg.sender, eventName, quantity);
    }

    function redeemTicket(string memory eventName, uint256 quantity) external {
        require(userTickets[msg.sender][eventName] >= quantity, "Not enough tickets to redeem");

        userTickets[msg.sender][eventName] -= quantity;

        emit TicketRedeemed(msg.sender, eventName, quantity);
    }

    function transferTicket(address to, string memory eventName, uint256 quantity) external {
        require(userTickets[msg.sender][eventName] >= quantity, "Not enough tickets to transfer");

        userTickets[msg.sender][eventName] -= quantity;
        userTickets[to][eventName] += quantity;

        emit TicketTransferred(msg.sender, to, eventName, quantity);
    }

    function checkTicketBalance(address account, string memory eventName) external view returns (uint256) {
        return userTickets[account][eventName];
    }
}
