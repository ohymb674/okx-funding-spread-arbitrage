# OKX Arbitrage Bot: Funding Rates or Spreads? What Each Mode Pays, How to Set It Up, and Three Ways You Can Still Lose Money (With a Worked BTC Example)

OKX's arbitrage bot isn't one strategy. Inside the bot list, the entry called **Arbitrage order** splits into two modes: **Funding rates**, which collects perpetual funding payments against a spot position, and **Spreads**, which trades the price gap between two instruments. Which one you want depends on whether the funding rate on your pair is positive or negative, and whether you're willing to hold the spot leg while the position is open.

Below: how the two modes differ, the exact click path for each, a worked BTC spread example with real numbers, the three places this tool can still cost you money, and honest answers on fees and minimums (the documentation doesn't publish them, and I won't invent them).

> **Quick verdict:** start with **Funding rates**. Open the portfolio list and read the two columns OKX puts there, expected APYs and current funding rates, before you size anything. If funding is positive, you buy spot and short the perpetual. If it's negative, the trade reverses and the setup most tutorials show will cost you money.
>
> 👉 [Open OKX's Arbitrage order bot and check live funding rates](https://okx.com/join/CASH20)

## Funding rates vs Spreads: what actually differs

| Mode | What pays you | Position structure | Main risk | Suits |
| --- | --- | --- | --- | --- |
| **Funding rates** | Perpetual funding payments | Spot position plus an offsetting perpetual position on the same asset | Funding flips sign for a long stretch and you start paying | Traders willing to hold the spot leg and collect a carry |
| **Spreads** | The price gap between two instruments | Opposite positions in spot and futures, or in two futures contracts | The futures leg gets liquidated before settlement | Traders targeting a specific gap rather than a holding yield |

Both modes live under the same roof. You select **Arbitrage order** from the list of trading bot strategies, then tap the trading pairs at the top to choose which bot you want.

## Funding rate arbitrage: the full setup

Each funding rate portfolio pairs one spot market with the equivalent perpetual contract. When you're picking a portfolio, OKX shows expected APYs and the current funding rate for each one. That column is the whole decision.

1. Select **Arbitrage order** from the bot strategies list.
2. Tap the pair selector at the top, then choose **Funding rates**.
3. Pick your settlement currency: **Crypto-margined** to settle in the coin you're trading, or **USDT-margined** to settle in USDT.
4. Choose the portfolio. The bot decides which leg buys and which leg sells based on that portfolio.
5. Enter the USD price and the amount for each leg.
6. Pick a margin mode. **Cross** uses all account funds of the currency traded as the position's margin. **Isolated** uses only the funds you designate as margin.
7. Review the details and hit **Both Legs** to submit both orders at once.

**The direction rule is the part people get wrong.** If the funding rate is positive, you want to be short the perpetual, because longs pay shorts to keep the position open. If the funding rate is negative, you want to be long the perpetual so you collect from the short side. Copying "buy spot, short perp" as a fixed recipe means that in a negative funding environment you're the one paying the other side every settlement.

You can override which leg buys and which sells. OKX explicitly does not recommend that less-experienced traders deploy a custom portfolio like that, and the reason is simple: the default pairing is what makes the two legs offset each other.

👉 [Set up your first funding rate pair on OKX](https://okx.com/join/CASH20)

## Spread arbitrage: the setup and the math

Spread arbitrage takes opposite positions in two markets and profits from the difference. On the current interface you select **Spreads**, then choose between **USDT-margined** and **USDC-margined**, pick your strategy, set the prices for both legs, choose **Cross** or **Isolated** margin, and place both orders.

The worked example OKX uses is worth walking through, because it shows why the trade is indifferent to where price ends up:

You buy 1 BTC at $50,000 on spot and sell a futures contract at $50,100.

| At settlement | Spot leg | Futures leg | Net |
| --- | --- | --- | --- |
| BTC at $55,000 | +$5,000 | −$4,900 | Just under $100, before fees |
| BTC at $45,000 | −$5,000 | +$100 locked in | Just under $100, before fees |

You captured the $100 gap that existed when you entered. Direction doesn't change the outcome. Fees do, and the example is stated net of them both times.

The order ticket has more controls than the funding side. You can type a **Spread rate** to set the target gap between the two legs directly, and choose between **Queuing**, **Surpassed**, **BBO** and **Market** order types. The **Set price** menu adjusts the Queuing and Surpassed parameters, and those two types create a fill range so both legs land at favorable prices.

One checkbox deserves attention. It places a market order on the second leg the moment the first leg fills, which guarantees both legs get done. It can also shrink the spread you capture, which eats directly into the profit you entered the trade for. Guaranteed execution and a wide spread are competing goals on that ticket; decide which one you need before you tick it.

## Three places you can still lose money

**1. Funding turns against you and stays there.** OKX calls the funding strategy low risk because the two positions cancel each other out. That's not the same as no risk. If the funding rate goes against you for a prolonged period, you pay the opposite side of the perpetual position and may lose money overall. A carry trade that pays 0.01% per settlement is lovely until it costs 0.05% for two weeks.

**2. Leverage on either leg.** The multiplier option raises potential profit against your starting capital and raises liquidation risk with it. On a strategy whose entire premise is that the two legs offset, adding leverage to the derivatives side is how you break that premise.

**3. The futures leg gets liquidated, not the spread.** For spread arbitrage, OKX states the only real risk plainly: the price moves so much that your futures position is liquidated. The gap converging is not the problem. Being forced out of the futures leg before settlement is, because at that point the hedge is gone and the spot leg is naked.

👉 [Compare funding rates across OKX portfolios before sizing a position](https://okx.com/join/CASH20)

## Smart arbitrage is a different product

The bot list also carries **Smart arbitrage**, which is not the same entry as Arbitrage order. It runs a delta-neutral position: equal-sized, opposite positions in spot and perpetual, so price moves cancel and the return comes from funding rather than price. OKX's own illustration is buying SOL on spot while selling the same amount of SOL perpetual contracts.

If you're comparing bots in the marketplace, check which of the two you're actually looking at. They solve a similar problem through different interfaces, and the settings don't transfer between them.

## Fees, minimums, and automation: what the documentation actually says

**Does OKX charge for the arbitrage bot?** The published material states profits net of trading fees in every worked example, but it does not print a fee schedule for this tool. Check the fee tier on your own account before you commit capital, and treat any article quoting a specific rate for the bot as unverified.

**Is there a minimum order size?** Not stated in the documentation reviewed. It will depend on the pair and the instrument, so confirm on the order ticket.

**Can I automate it through the API?** The OKX help page titled "How do I automate strategies on Arbitrage bot?" walks through the manual click path only: select the strategy, fill in order details, set margin mode, place both orders. No API parameters or programmatic controls appear there. If you need rule-driven execution without clicking, this documented tool doesn't offer it.

**How do I close a position?** Go to the trade history section at the bottom of the trading screen, tap **Bot**, then **Arbitrage order**, where current profit and open positions are listed.

**Which margin mode should I pick?** Cross uses all account funds in that currency as margin, which is more forgiving on small drawdowns and more dangerous if the position goes bad. Isolated caps the damage at the amount you allocated. If you're learning the tool, isolated keeps the blast radius visible.

## Picking between the two

Positive funding on a pair you're comfortable holding spot for: **Funding rates**. You're being paid a carry to hold a hedged position, and the portfolio list tells you the current rate before you commit.

An unusual gap between two instruments that you expect to close: **Spreads**. You're trading a specific number, and the $50,000/$50,100 example shows the outcome doesn't depend on direction.

Never traded futures or perpetual swaps: read OKX's tutorials on both products before touching either mode. That's the platform's own recommendation, and it's the right order of operations, since both modes put you in a derivatives position on day one.

Two things to confirm on the ticket before you press Both Legs: the sign of the funding rate, and which margin mode is selected. Those two settings decide whether the strategy pays you or you pay it.

👉 [Create an OKX account and open Arbitrage order](https://okx.com/join/CASH20)
