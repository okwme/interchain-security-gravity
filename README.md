# Gravity Delegations Tool

### Environment Variables

the following Environment variables need to be configured

| Key        | Default |
| ---------- | ------- |
| GRANTER    | alice   |
| GRANTEE    | bob     |
| BINARY     | simd    |
| DENOM      | stake   |
| MAX_AMOUNT |         |

if MAX_AMOUNT is not set the total amount currently held by the granter will be delegated

### Executables

Make sure the file is executable

```bash
chmod +x gen.py
```

Make sure the delegation script is executable

```bash
chmod +x delegate.sh
```

### Prerequisites

- Ensure a list of validators can be found at the root dir named `validators.json`
- Install binary of chain in question and join their mainnet (statesync using snapsnots is recommended)
- In order to allow for smooth functioning of this script a GRANTEE needs to be configured

This key requires the following permissions

- MsgDelegation

```bash
$BINARY tx authz grant $($BINARY keys show $GRANTEE -a) generic --msg-type=/cosmos.gov.v1beta1.MsgDelegation --from $GRANTER --fees 200$DENOM
```

- FeeGrant

```bash
$BINARY tx feegrant grant $(BINARY keys show $GRANTER -a) $(gaiad keys show $GRANTEE -a) --spend-limit 10000uatom --expiration 2023-01-30T15:04:05Z --fees 200$DENOM
```

### Running the delegation script

Use the command as follows:

```bash
./delegate.sh
```

The script should withdraw all rewards for the GRANTER
Divide the current balance of the Delegator by the number of validators they plan to delegate to
Generate the message for this TX
Make an authz tx sing the GRANTEE to sign the transaction

### Running python script

NOTE `delegate.sh` is a wrapper for gen.py, thus if you use it there is no need to run the script directly

```bash
python3 gen.py
```

For help try the `--help` flag:

```bash
> python3 gen.py --help
usage: gen.py [-h] [--amount AMOUNT] [--from-address FROM_ADDRESS] [--output-file OUTPUT_FILE]

Delegate to Cosmos Hub validators

optional arguments:
  -h, --help            show this help message and exit
  --amount AMOUNT       Amount to delegate to each
  --from-address FROM_ADDRESS*
                        Address to send grav from (default new-new-bom)
  --output-file OUTPUT_FILE
                        Filename to write
```

Default values for the flags are as follows:

- `--amount` = 1
- `--from-address` = `gravity1zk4uwzygs4k2k5cz7y759sxcnv6qt3pd4g3pqq` (new-new-bom)
- `--output-file` = today's date in format `%d-%m-%Y/unsigned.json`

Examples:

```bash
python gen.py --amount 1000000000000
python gen.py --amount 1000000000000 --from $(gravity keys show new-new-bom -a)
python gen.py --amount 1000000000000 --output-file output/unsigned.json
```
