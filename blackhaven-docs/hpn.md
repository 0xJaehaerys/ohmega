# Haven Protected Notes (HPN)

Haven Protected Notes (HPNs) are principal-protected positions issued as ERC-721 NFTs, designed to accrue yields and MegaETH ecosystem rewards. Users deposit stable assets (USDM or USDMy) for a chosen commitment term, minting a unique NFT that records the principal amount, terms, and ongoing rewards. Principal is fully redeemable at maturity with no market exposure, and holders can redeem RBT directly against the HPN or exit early to recover initial principal (subject to a small fee and forfeiture of unvested accruals).

Blackhaven's user-first model ensures clear principal redemption while deploying deposits across vetted strategies to generate yields and earn rewards. These returns are allocated directly to depositors (with amplified MEGA Points via TVL contributions unavailable outside of Blackhaven) and reserves. 

**Core Mechanics**

| Component | Description |
| --- | --- |
| Principal Protection | Full redemption of principle. |
| Deposit Assets | USDM or USDMy *(current MegaETH whitelisted assets).* |
| NFT Issuance | A unique ERC-721 is minted to represent the position. |
| Commitment Period | Dynamic user-selected duration. |
| Yield Accrual | Ecosystem yield and MegaETH points. |
| RBT Redemption | Redeem RBT against the HPN based on initial principal, burning the NFT upon completion. |
| Early Redemption | Allowed with a small exit fee and forfeiture of all rewards. |
| Composability | NFT is on-chain and compatible with broader ecosystem integrations. |

<aside>
💡

Haven Protected Notes are not market-priced instruments. Their value is defined solely by the principal, accrued rewards, and commitment terms, ensuring a predictable and protected user experience.

</aside>

### How HPNs Work: User Options

- **Hold to Maturity**
    
    Hold the HPN through its term. The position accrues yield and MEGA points. At maturity, redeem full principal plus accrued rewards.
    
- **Early Redemption**
    
    Exit at any time and recover full initial principal minus a small fee. Accrued yield and MEGA points are forfeited to the treasury.
    
- **RBT Redemption**
    
    Redeem the HPN for RBT equal to the initial principal; the HPN burns on redemption and accrued yield and MEGA points are forfeited to the treasury.
    

![Flowchart.jpg](Flowchart.jpg)

<aside>
💡

At maturity or early exit, the HPN enters a 7-day cooldown, after which the position becomes claimable.

</aside>
