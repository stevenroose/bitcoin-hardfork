List of Bitcoin enhancements that require a hard-fork
==========

Since the whole Bitcoin community seems to be so enraged that the block size 
needs to be increased that they are organizing hard-forks mostly for this 
feature only 
([BitcoinXT](https://bitcoinxt.software), 
[Bitcoin Classic](https://bitcoinclassic.com)).
Instead of focusing so hard on block size, it might be a better idea to have a 
complete list of enhancements that we might want in the near future and that 
also require a hard-fork.
Because the process of a hard-fork is so long and cumbersome, we might want to 
include as much as possible at one time.

**This document is an effort to draft a list of enhancements that should be 
implemented in Bitcoin using a hard-fork, preferably in the near future. All 
contributions, both additions as critics are welcome!**


# 1. Block size increase

We all want bigger blocks. Even the authors of The Lightning Network, often 
blamed for delaying progress towards a block size increase, know that bigger 
blocks are imminent. 
>"While it may appear as though this system will mitigate the block size
increases in the short term, if it achieves global scale, it will necessitate a
block size increase in the long term."

There are several proposals for a concrete implementation of a block size 
increase, most of which have been described in a BIP:
- [BIP 101: Increase maximum block size][bip101] 
by Gavin Andresen (Core dev, BitcoinXT, Bitcoin Classic)
- [BIP 102: Block size increase to 2MB][bip102] 
by Jeff Garzik (Core dev, Bitcoin Classic)
- [BIP 103: Block size following technological growth][bip103] 
by Pieter Wuille (Core dev, Blockstream)
- [BIP 105: Consensus based block size retargeting algorithm][bip105] 
by BtcDrak
- [BIP 106: Dynamically Controlled Bitcoin Block Size Max Cap][bip106] 
by Upal Chakraborty
- [BIP 107: Dynamic limit on the block size][bip107] 
by Washington Y. Sanchez (OpenBazaar)

There doesn't seem to be consensus in the community towards a preferred solution
amongst the alternative implementations. Even though we do need a single 
preferred solution. More so, we want a solution that is good for the long run,
not one that solves block space shortage now but will pose a similar problem 
somewhere in the future.

[bip101]: https://github.com/bitcoin/bips/blob/master/bip-0101.mediawiki
[bip102]: https://github.com/bitcoin/bips/blob/master/bip-0102.mediawiki
[bip103]: https://github.com/bitcoin/bips/blob/master/bip-0103.mediawiki
[bip105]: https://github.com/bitcoin/bips/blob/master/bip-0105.mediawiki
[bip106]: https://github.com/bitcoin/bips/blob/master/bip-0106.mediawiki
[bip107]: https://github.com/bitcoin/bips/blob/master/bip-0107.mediawiki


# 2. Segregated Witness _(\*)_

[Segregated Witness][segwit] defines a new structure called a "witness" that is 
committed to blocks separately from the transaction merkle tree. This structure 
contains data required to check transaction validity but not required to 
determine transaction effects. In particular, scripts and signatures are moved 
into this new structure. 
Segregated Witness is currently implemented in [Elements Alpha][elements].

By removing this data from the transaction structure committed to the 
transaction Merkle tree, several problems are fixed:
1. Nonintentional malleability becomes impossible.
2. Transmission of signature data becomes optional.
3. Some constraints could be bypassed with a soft fork.

Even though Segregated Witness could be implemented using a soft-fork,
by using part of the coinbase transaction. However, this is not what the 
coinbase transaction is intended for and when a hard-fork is imminent, encoding 
this directly into the block header would be a better solution.

[segwit]: https://github.com/bitcoin/bips/blob/master/bip-0141.mediawiki
[elements]: https://github.com/ElementsProject/elements


# 3. Confidential Transactions _(\*)_

Outlined [by Gregory Maxwwell here][ct], 
Confidential Transaction allows for keeping amounts in Bitcoin transactions 
private.
Unlike other proposals for more transaction privacy, Confidential Transactions 
does not break any other important features like pruning and existing 
techniques like CoinSwap not does it incur a high computational overhead.
As a side-effect it also enables adding a private memo to each transaction,
which is a useful feature for adding payment-related data to your transaction.

Confidential Transactions are already implemented in [Elements Alpha][elements] 
and they are performing great.

[It has been shown][ct-softfork] that Confidential Transactions can be 
implemented as a soft-fork. However, the implementation is not elegant and 
poses several additional requirements for miners.

[ct]: https://people.xiph.org/~greg/confidential_values.txt
[elements]: https://github.com/ElementsProject/elements
[ct-softfork]: https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2016-January/012194.html


# 4. Block header commitments for compact SPV proofs

The sidechain paper the Blockstream devs et al. outlines an optimization that 
would allow for the creation of much more compact SPV proofs and much shorter
generation times for these proofs. It requires miners to commit to more than
just the previous block in their block header. By including a Merkle root of 
multiple (all?) previous blocks into their block headers.

As the Bitcoin network grows, it is logical to assume that the number of SPV
clients will increase. Therefor, a lot of clients will be generating SPV proofs
in the future, which would all benefit from a more efficient SPV scheme. Also
compacter SPV proofs would allow smaller pegging transactions for moving coins
between sidechains.

Mark Friedenbach has already [proposed][compact-spv-softfork] a scheme for 
doing this by placing the commitment in the coinbase transactions. 
Again, this is not what the coinbase transaction is intended for and when
a hard-fork is imminent, encoding this directly into the block header would be
better.

[compact-spv-softfork]: https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2014-March/004727.html]



# Others? 

This is just a list of thing I have in mind. I am sure that others have 
more features that should be included or think differently about the ones I 
listed. 


---------

 _(\*)_: Features marked _(\*)_ are already implemented in Elements Alpha.
