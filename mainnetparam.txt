var genesisBlock = wire.MsgBlock{
	Header: wire.BlockHeader{
		Version:      1,
		PrevBlock:    chainhash.Hash{},
		MerkleRoot:   genesisMerkleRoot,
		StakeRoot:    chainhash.Hash{},
		Timestamp:    time.Unix(1509076800, 0), // Fri, 27 Oct 2017 12:00:00 GMT
		Bits:         0x1d0fffff,               // Difficulty 32767
		// Bits: 0x207fffff,
		SBits:        2 * 1e8,                  // 2 Coin
		Nonce:        0x00000000,
		StakeVersion: 0,
	},
	Transactions: []*wire.MsgTx{&genesisCoinbaseTx},
}










var MainNetParams = Params{
	Name:        "testdata2",
	Net:         wire.MainNet,
	DefaultPort: "14008",
	DNSSeeds: []string{
		"testnet-seeds.hcashtech.org",
	},

	// Chain parameters
	GenesisBlock:             &genesisBlock,
	GenesisHash:              &genesisHash,
	PowLimit:                 mainPowLimit,
	DifficultyRate:           16,
	MaxMicroPerKey:           31,
	PowLimitBits:             0x1d0fffff,
	// PowLimitBits: 0x207fffff,
	ReduceMinDifficulty:      false,
	MinDiffReductionTime:     0, // Does not apply since ReduceMinDifficulty false
	GenerateSupported:        false,
	MaximumBlockSizes:        []int{2048000},
	MaxTxSize:                2048000,
	TargetTimePerBlock:       time.Minute * 5,
	WorkDiffAlpha:            1,
	WorkDiffWindowSize:       144,
	WorkDiffWindows:          20,
	TargetTimespan:           time.Minute * 5 * 144, // TimePerBlock * WindowSize
	RetargetAdjustmentFactor: 4,

	// Subsidy parameters.
	BaseSubsidy:              5000000000, //
	MulSubsidy:               1000,
	DivSubsidy:               1005,
	SubsidyReductionInterval: 1543,
	WorkRewardProportion:     45,
	StakeRewardProportion:    45,
	BlockTaxProportion:       10,

	// Checkpoints ordered from oldest to newest.
	Checkpoints: []Checkpoint{
	//	{30,newHashFromStr("00000df9e4054bd941145c7ea9dbefc29e47ed564cc2fdb254720ab07a016938")},
	//	{200,newHashFromStr("00000019ed43fba03c72b03cbd7a706c50b56819379b478da57184363fd90a68")},
		},

	// The miner confirmation window is defined as:
	//   target proof of work timespan / target proof of work spacing
	RuleChangeActivationQuorum:     4032, // 10 % of RuleChangeActivationInterval * TicketsPerBlock
	RuleChangeActivationMultiplier: 3,    // 75%
	RuleChangeActivationDivisor:    4,
	RuleChangeActivationInterval:   2016 * 4, // 4 weeks
	Deployments: map[uint32][]ConsensusDeployment{
		4: {{
			Vote: Vote{
				Id:          VoteIDSDiffAlgorithm,
				Description: "Change stake difficulty algorithm as defined in DCP0001",
				Mask:        0x0006, // Bits 1 and 2
				Choices: []Choice{{
					Id:          "abstain",
					Description: "abstain voting for change",
					Bits:        0x0000,
					IsAbstain:   true,
					IsNo:        false,
				}, {
					Id:          "no",
					Description: "keep the existing algorithm",
					Bits:        0x0002, // Bit 1
					IsAbstain:   false,
					IsNo:        true,
				}, {
					Id:          "yes",
					Description: "change to the new algorithm",
					Bits:        0x0004, // Bit 2
					IsAbstain:   false,
					IsNo:        false,
				}},
			},
			StartTime:  1493164800, // Apr 26th, 2017
			ExpireTime: 1524700800, // Apr 26th, 2018
		}, {
			Vote: Vote{
				Id:          VoteIDLNSupport,
				Description: "Request developers begin work on Lightning Network (LN) integration",
				Mask:        0x0018, // Bits 3 and 4
				Choices: []Choice{{
					Id:          "abstain",
					Description: "abstain from voting",
					Bits:        0x0000,
					IsAbstain:   true,
					IsNo:        false,
				}, {
					Id:          "no",
					Description: "no, do not work on integrating LN support",
					Bits:        0x0008, // Bit 3
					IsAbstain:   false,
					IsNo:        true,
				}, {
					Id:          "yes",
					Description: "yes, begin work on integrating LN support",
					Bits:        0x0010, // Bit 4
					IsAbstain:   false,
					IsNo:        false,
				}},
			},
			StartTime:  1493164800, // Apr 26th, 2017
			ExpireTime: 1508976000, // Oct 26th, 2017
		}},
	},

	// Enforce current block version once majority of the network has
	// upgraded.
	// 75% (750 / 1000)
	// Reject previous block versions once a majority of the network has
	// upgraded.
	// 95% (950 / 1000)
	BlockEnforceNumRequired: 750,
	BlockRejectNumRequired:  950,
	BlockUpgradeNumToCheck:  1000,

	MicroBlockValidationHeight: 64,

	// Mempool parameters
	RelayNonStdTxs: false,

	// Address encoding magics
	NetworkAddressPrefix: "H",
	PubKeyAddrID:         [2]byte{0x19, 0xa4}, // starts with Hk
	PubKeyHashAddrID:     [2]byte{0x09, 0x7f}, // starts with Hs
	PKHEdwardsAddrID:     [2]byte{0x09, 0x60}, // starts with He
	PKHSchnorrAddrID:     [2]byte{0x09, 0x41}, // starts with HS
	ScriptHashAddrID:     [2]byte{0x09, 0x5a}, // starts with Hc
	PrivateKeyID:         [2]byte{0x19, 0xab}, // starts with Hm

	// BIP32 hierarchical deterministic extended key magics
	HDPrivateKeyID: [4]byte{0x02, 0xfd, 0xa4, 0xe8}, // starts with dprv
	HDPublicKeyID:  [4]byte{0x02, 0xfd, 0xa9, 0x26}, // starts with dpub

	// BIP44 coin type used in the hierarchical deterministic path for
	// address generation.
	HDCoinType: 20,

	// Hypercash PoS parameters
	MinimumStakeDiff:        2 * 1e8, // 2 Coin
	TicketPoolSize:          8192,
	TicketsPerBlock:         5,
	TicketMaturity:          128/*256*/,
	TicketExpiry:            40960, // 5*TicketPoolSize
	// CoinbaseMaturity:        128/*256*/,
	CoinbaseMaturity:        1,
	SStxChangeMaturity:      1,
	TicketPoolSizeWeight:    4,
	StakeDiffAlpha:          1, // Minimal
	StakeDiffWindowSize:     144,
	StakeDiffWindows:        20,
	StakeVersionInterval:    144 * 2 * 7, // ~1 week
	MaxFreshStakePerBlock:   20,          // 4*TicketsPerBlock
	StakeEnabledHeight:      128 + 128/*256 + 256*/,   // CoinbaseMaturity + TicketMaturity
	StakeValidationHeight:   512,        // ~14 days
	StakeBaseSigScript:      []byte{0x00, 0x00},
	StakeMajorityMultiplier: 3,
	StakeMajorityDivisor:    4,

	// Hypercash organization related parameters
	// Organization address is HcR2g1eGf6mpQz7QNa1u5AFeWq8yKnfRUeM
	OrganizationPkScript:        hexDecode("a914cc53f47615e01dcae979f5813278095627f095cc87"),  //HccmQdPB1hkFQcB7jE2drqRYbavEhoQuAdx
	OrganizationPkScriptVersion: 0,
	BlockOneLedger:              BlockOneLedgerMainNet,
}