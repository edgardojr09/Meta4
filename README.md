# Building on avalache
## Description of each statement and fucntion
Import - These import ERC20 token standard and Ownable contract from OpenZeppelin library. ERC20 provides standard functionalities for tokens, and Ownable provides a way to manage ownership
Conrtract - Define a new Contract 
Events - are declarations of events that can be emitted to notify external listeners about certain actions like creating, redeeming, or transferring tickets.
Mint - This function allows the owner to create new tokens and assign them to a specified address.
Burn - allows users to destroy a specified amount of their own tokens
Transfer - This function overrides the default transfer function to ensure it uses the sender’s address correctly.
CreateTicket - function allows the owner to create tickets for an event, specifying the event name, quantity, and price. It also emits a TicketCreated event
BuyTicket - This function allows users to buy tickets for a specified event, provided they have enough tokens to cover the total price. It burns the tokens and updates the ticket balances, then emits a TicketRedeemed event
ReedemTicket - This function allows users to redeem their tickets for an event. It decreases the user's ticket balance and emits a TicketRedeemed event
Balanceof - Checking always the balanceof
