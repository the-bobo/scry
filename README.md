# Prediction Markets for Developers

This guide offers a brief overview of prediction markets, geared towards developers interested in building them. This document **does not** provide legal advice. All risk assumed is your own.

# Intro
## What is a prediction market?
In a prediciton market (PM) participants place bets on the outcome of future events. They are rewarded if their predictions come true. The price of a contract reflects the market estimate of the probability that that event occurs. 
## What is it supposed to solve?
Prediction markets are supposed to generate accurate forecasts of future events' probabilities. I've seen two explanations for the mechanics by which PM's are theorized to act. The first is through **statistical averaging,** in which many noisy guesses are averaged to produce a more accurate, less noisy estimate.

The second is through **information elicitation,** in which asymmetric private information is converted into public information by rewarding PM participants with monetary gain. If some individuals have information others don't, then this information must be elicited to produce the most accurate forecast possible. PM's incentivize disclosing this information in the form of potential revenue: if a participant places the right bets she will make a profit.

PM's are useful when we want to know the likelihood of an event before it occurs. See Appendix A: Mechanical Explanations for more theories about how and why PM's might work.
## Legality
This document **cannot** provide legal advice. All consequences and risk remain that of the implementer. The following is just my best guess, non-lawyer understanding of what's what.

In general, online gambling is illegal in the United States. Two markets have special exceptions. The Iowa Electronic Markets (founded 1988) and PredictIt (founded 2014) both have "no-action" letters from the Commodity Futures Trading Commission (CFTC). These letters state the CFTC does not intend to pursue action against the markets in question.

Trades in both markets are constrained as a result, with max-wagers in the neighborhood of $850 or $500 USD. Other restrictions concerning account sign-up and verification may also apply. These markets are academic markets, intended for research into prediction market mechanics.

In general, online PM's with *fake money* or points appear to be legal. A well-known example is the Hollywood Stock Exchange (HSX). 

In some cases, such as online sports fantasy leagues, playing with points that can later be cashed out for real money may or may not be legal.
## Ethics
PM's are powerful tools because they incentivize participation with the potential to make a profit. Sometimes PM's may be deemed unethical, as in the case of the FutureMAP project, a Pentagon prediction market for economic, civil, and military outcomes in the Middle East. Congressmen denounced FutureMAP as a ["terrorism futures" market](http://www.nytimes.com/2003/07/29/us/threats-responses-plans-criticisms-pentagon-prepares-futures-market-terror.html), with the rationale that markets that permit betting on the occurrence of violent acts may provide a financial incentive for such acts to occur. It is worth noting that the one of the earliest proponents of prediction markets, Robin Hanson, was involved in the creation of FutureMAP, and [rejects the Congressmen's characterization](http://mason.gmu.edu/~rhanson/policyanalysismarket.html) of the market, [as does David Pennock](http://dpennock.com/pam.html), another noted prediction markets researcher. 

# How do prediction markets work? 
## Key players
In PM's there are three key entities: participants, the market institution (or "market maker"), and the oracle. Participants place bets on the outcome of future events. Market institutions facilitate the bet-making process, keep track of bets, and handle payment. Oracles are a source of turth that determines whether or not an event happened. Participants agree to be bound by oracles' decisions when placing their bets with the market maker.
## Making a bet: two models
There are two dominant models for how betting can be coordinated in a PM. We'll walk through one to motivate the other. 

In any PM the price of a contract reflects the market participants' belief in the likelihood of the event occuring (see also: Arrow-Debreu securities). Prices are generally between $0.01 and $0.99, with a successful contract paying $1 upon completion. Accordingly, if you buy a Yes share in an event at $0.05 and the event later happens, you make $0.95 in profit. If you had waited until the price was $0.50, your profit would only be $0.45. 

How should we set the price of shares?
## Pricing model one: Continuous Double Auction (CDA)
A continuous double auction (CDA) requires both sides of the contract (the "yes" side and the "no" side) to clear for a transaction to take place. In a CDA, a buyer makes a bid for how much they are willing to pay for a given side of a particular contract. Sellers make "asks" for how much they are willing to sell a given side of a given contract.

For example, a buyer may make a bid of $0.05 on the "yes" side of the contract "Will it rain tomorrow?" If the market institution cannot find any seller willing to sell the Yes share at $0.05 (equivalent to buying a No share at $0.05), the buyer's trade will not clear. Similarly, sellers can make asks, advertising a share at a price of their choosing. Such markets will often show the closest bid/ask matches to a prospective buyer/seller, permitting them to change their offer to close a trade.

The big problem with the CDA model is a lack of market liquidity (ability to close trades) because of a wide "bid/ask spread." If buyers and sellers do not agree on the price, the market cannot close enough trades to operate healthily. If a market is not healthy it cannot attract more buyers and sellers. Low participation makes participating hard, and making participation hard makes attracting participants difficult.
## Pricing model two: Market Scoring Rule (MSR)
What if the market institution could offer the trader a price without requiring a matching trade in the opposite direction? This approach relies on using a "market scoring rule" (MSR, invented by Robin Hanson) to determine the price of contracts. In this world, every trader could trade instantaneously, so the market would always stay liquid. The friction to participating would be minimal for the user, perhaps resulting in a healthier market.

Sound to good to be true? It does come at a cost - to the market maker. The market institution must be willing to risk a (bounded) amount of money. This is a finite, worst-case amount of money the market institution could lose if all the bets went against it. In return for risking this money, the market maker can ensure a liquid market for participants. Fortunately, this amount is dependant upon the selection of a parameter (the "liquidity parameter," which is equivalent to the "learning rate" in generic machine learning algorithms: it determines how quickly the price adjusts to new trades being made) that the market maker gets to choose. In combination with transaction fees, the market maker could hope to recoup or even make a profit against the money it has to front to ensure liquidity.

After his pioneering work on automated market maker mechanisms Hanson became a consultant, helping private companies build internal prediction markets with his company [ConsensusPoint](http://www.consensuspoint.com). Hanson is now an Advisor to both [Augur.net](www.augur.net) and [Gnosis.pm](www.gnosis.pm), two blockchain-based prediction market startups.

See Appendix B: Scoring Rules for a closer look at what a scoring rule is, what a strictly proper scoring rule is, and how the loss is bounded to the market institution in a later section.

# Example prediction markets
The following descriptions are not exhaustive.
##Iowa Electronic Markets
*Unregulated by CFTC or other agencies ("no-action" letter)
*Real-money
*Not-for-profit
*Academic intent
*Participants may only risk between $5 and $500 
*Operated by University of Iowa Tippie College of Business
##PredictIt
*Unregulated by CFTC or other agencies ("no-action" letter)
*Real-money
*Not-for-profit
*Academic intent
*Participants may only risk up to $850 per question
*Limit of 5,000 traders per question
*No participation from residents of Nevada or Washington state
*Operated by Victoria University of Wellington
##Nadex
*"North American Derivatives Exchange"
*Regulated by CFTC
*Real-money
*Trades must be fully funded in advance. No margin trading.
##Augur
*Decentralized prediction market built on top of Ethereum, a blockchain technology (like Bitcoin, but different)
*Oracle function is distributed, and operates on the basis of "REP." Oracles are incentivized to be truthful by receiving a share of transaction fees on the basis of how much REP they control. Deceitful Oracles lose REP, which is redistributed to truthful Oracles.
##Gnosis
*Decentralized prediction market built on top of Ethereum
*Oracle function is a hybrid decentralized/centralized approach, with the goal of clearing trades faster than Augur (which could take a month to allow its distributed Oracles to complete voting)
*See https://blog.gnosis.pm/the-difference-between-gnosis-and-augur-c08077271a8e for more
##Hollywood Stock Exchange (HSX)
*Fake money (points)
*Targeted at entertainment industry, e.g. predicting Oscars
*Has a history of performing well, according to its Wikipedia page. Unclear about recent performance.
##Predictious
*Bitcoin prediction market
*Appears to be low volume, low liquidity (blog posts are from 2014)
*Incorporated in Ireland

# Example prediction market software
Unfinished
*Zocalo (not maintained, out of date)
*https://github.com/nikete/MarketMaker

# Desirable theoretical properties of a PM
There are several desirable theoretical properties for a PM.
1. Outcomes are mutually exclusive - contracts should cover mutually exclusive outcomes: e.g., it either rains tomorrow or it does not.
2. Strategy proofness - A strategy proof market is one in which participants make bets that reflect their true estimates of an outcome's probability. In technical langauge, we want "myopic incentive compatibility," which means that an agent who comes to the market once to place a single bet on a given question reports their true belief. Strategy proofness over the long-run (e.g. multiple bets by the same participant, perhaps in multiple questions, perhaps in an attempt to game other participants) may be a more difficult condition to meet.
3. Arbitrage-free (under condition of rational agent) - Abritrage refers to a method of extracting value from the market without contributing information. An aribtraging player starts with a certain amount of money, e.g. $100, makes a certain sequence of trades, and ends up with more money *without having risked any loss.* We want to avoid arbitrage opportunities, as they represent inefficiencies in the market. Transaction fees are one method of imposing an opportunity cost on arbitraging players.
4. Sybil Proofness (under condition of rational agent) - A Sybil attack refers to a malicious player attacking a system by signing up for multiple accounts. Accounts that might look to be distinct are actually all under the control of a single player. This attack is not expected to matter for real-money markets, where the value of 10 accounts with $5 each is the same as the valu eof 1 account with $50. Sybil attacks may pose a greater challenge for fake money (points) markets, where account creation is incentivized with a starting reward of some initial amount of points (UNFINISHED - how does HSX deal with this?)
5. Trustworthy Oracles - how can market participants trust the Oracles? How might Oracles participation in the market threaten Oracle integrity?
6. Bounded loss to market maker - If we are using an automated market maker mechanism, we expect to have a known, finite worst-case loss to the market maker.

# Practically speaking, how can a PM be compromised?
Use this section and the above to aid production of your threat model. 
1. Sybil attacks - if it's play money, and you get some play money for signing up, then you incentivize false account signup. 

2. Price manipulation - if the market is taken as informative by outsiders, then incentive exists to manipulate the prices (which are probabilities that the event you're betting on will happen)

3. Strategic betting - markets may be susceptible to agents lying about what they think the probabilities are, in order to manipulate other bettors

4. Arbitrage - some bettors may find arbitrage opportunities, in which they effectively win risk-free profit. In market terms, this translates to them adding no additional information to the market, but taking money out. Reward without risk.

5. Oracle co-optation - if the Oracle is allowed to bet (or can bet through a Sybil attack), they may skew the market in their favor

# Tracing a bet from start to finish
Unfinished
## CDA model
Unfinished
## MSR model
Unfinished

# A sample software architecture
Unfinished
*Explanation of microservice architecture
*what should be made into what microservice?
	*payment processing should be a standalone module that other microservices can call 
		to - helps w/ three possibilities for payment: fake points, cryptocurrency, or fiat currency
*concurrency - how do we resolve multiple bets being placed at the same time? If it's continuous double auction this looks different from automatic market maker.
	*what should the DB look like?
	*good concurrency language at scale - go? rust? not Cpython b/c global interpreter lock?

# Unfinished thoughts
IS rational agent assumption the right one?
		-Probably not, but maybe it doesn't matter too much: http://yiling.seas.harvard.edu/wp-content/uploads/socpm.pdf
		-Another model: BAR (byzantine, altruistic, rational, e.g., troll, goody two-shoes, 
		mercenary)

# Appendix A: Mechanical Explanations
Unfinished
*http://www.nature.com/news/the-power-of-prediction-markets-1.20820
*Dumb Agent Theory
*Wisdom of Crowds
*Opinion Pools (no information elicitaiton - http://www.cse.wustl.edu/~mithunchakraborty/papers/MSR_OP_NIPS15.pdf)

# Appendix B: Scoring Rules
Unfinished
*what a scoring rule is, what a strictly proper scoring rule is, and how the loss is bounded to the market institution

# References
*[PredictIt: How It Works](https://www.predictit.org/About/HowItWorks)
*[David Pennock's "Implementing Hanson's Market Maker" (joint work with Yiling Chen)](http://blog.oddhead.com/2006/10/30/implementing-hansons-market-maker/)
*[Augur Blog: "What is Reputation?" (Augur's distributed oracle approach)](http://augur.strikingly.com/blog/what-is-reputation)
*[A Utility Framework for Bounded-Loss Market Makers Chen, Pennock](https://web.archive.org/web/20100715043447/http://www.yiling.seas.harvard.edu/files/MM_UAI_final.pdf)
