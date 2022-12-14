# Integration Checklist

This checklist is here to ease integrations of Morpho and make them safer.

## General Comments

- [ ] Make sure the contract calling Morpho's core contracts or Morpho's vaults has the capacitiy to withdraw ERC20 tokens from the contract. Else, MORPHO rewards or the underlying pool's rewards would be stuck in the contract (forever...).
- [ ] Make sure the correct `Morpho` or `Lens` interface are used (they differ a bit between Morpho-Compound and Morpho-AaveV2).
- [ ] On Compound the cETH contract token does not behave the same way as the other cTokens (e.g. there's no `underlying()` getter, amount need to be passed as `msg.value`, ...).

## Morpho Core Protocol Integration

- [ ] Check that the interfaces you are using (Morpho and Lens) are up-to-date.
- [ ] You can repay/withdraw the whole debt/supply by passing `type(uint256).max` as argument to avoid leaving dust on Morpho.
- [ ] Positions on Morpho are not fungible. This means that you will not receive an interest bearing token as on Aave or Compound. However, Morpho will store your position in its storage. To get a fungible position on top of morpho, you'll need to use the Morpho's ERC4626-based vaults.
- [ ] On Morpho, it's allowed to supply on behalf of another address.

## Morpho ERC4626 Vaults Integration

- [ ] Check that the interfaces you are using (Morpho's vaults and Lens) are up-to-date.
- [ ] Vaults can only supply/withdraw assets into/from Morpho.
- [ ] ERC4626 has several arguments (`receiver` and `owner`), make sure to initialize them with the right values. Else, you risk to send your tokens to another address. Set `receiver == owner` in case the caller is supplying for itself.