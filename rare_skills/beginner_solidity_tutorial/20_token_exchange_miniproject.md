# Token Exchange mini project

We will build two ERC20 contracts: RareCoin and SkillsCoin.

Anyone can mint SkillsCoin, but the only way to obtain RareCoin is to send SkillsCoin to the RareCoin contract.

Requirements:

- Create `SkillsCoin` token, anyone can mint this token.
- Create `RareCoin` token. The only way to get this token is by trading SkillsCoin for it. When you call the trade function, the contract transfers the specified amount of SkillsCoin from the sender to itself and mints an equal amount of RareCoin to the sender.
