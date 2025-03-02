# Building Zefi App

## Create new leo Project:
```sh
leo new gcyadav_token_vesting_linear
```

## Setup project:
```sh
cd gcyadav_token_vesting_linear
leo add token_registry
leo add credits
```

## Modified `main.leo`:
- Paste below contents inside `main.leo` but change value of address with your public address.:

    <details><summary> Detailed Output </summary><blockquote>

    ~~~sh
    // The 'gcyadav_token_vesting_linear' program.
    import token_registry.aleo;
    import credits.aleo;

    program gcyadav_token_vesting_linear.aleo {
        const ADMIN: address = aleo1yhvl35uwjcy72hmacufn4h08ew7n7r97zq48vegqefuqtewh4grspu7gqk;

        record AccessRecord {
            owner:address,
        }

        struct TokenMetadata {
            token_id: field,
            name: u128,
            symbol: u128,
            decimals: u8,
            supply: u128,
            max_supply: u128,
            admin: address,
            external_authorization_required: bool,
            external_authorization_party: address
        }

        struct BeneficiaryMetadata {
            block_duration: u32,
            start_height: u32,
            number_of_interval: u32,
            total_amount: u128,
            claimed_amount: u128,
            token_id: field,  
        }
        mapping beneficiary_metadata:address => BeneficiaryMetadata;

        transition issue_access_record (to:address) -> AccessRecord {
            assert_eq(self.caller, ADMIN);
            return AccessRecord {
                owner: to
            };
        }


        mapping registered_token:field => bool;

        async transition register_token (token_id:field) -> Future {
            assert_eq(self.caller,ADMIN);
            return finalize_register_token(token_id);
        }

        async function finalize_register_token (token_id:field) {
            let token:TokenMetadata = token_registry.aleo/registered_tokens.get(token_id);
            let registered: bool = registered_token.get_or_use(token_id,false);
            assert(!registered);
            registered_token.set(token_id,true);
        }

        async transition deposit(beneficiary:address,amount:u128, block_duration:u32, start_height:u32, number_of_interval:u32, token_id:field ) -> Future {
            assert_eq(self.caller, ADMIN);
            let deposited:Future=token_registry.aleo/transfer_public(token_id, self.address, amount);
            let beneficiary_hash_address:address = BHP256::hash_to_address(beneficiary);
            return finalize_deposit(amount, beneficiary_hash_address , block_duration, start_height, number_of_interval,token_id,  deposited);
        }

        async function finalize_deposit (amount:u128, beneficiary: address, block_duration:u32, start_height:u32, number_of_interval:u32, token_id:field, deposited:Future) {
            deposited.await();
            let token:bool = registered_token.get(token_id);
            let metadata:BeneficiaryMetadata = beneficiary_metadata.get_or_use(beneficiary, BeneficiaryMetadata {
                block_duration: 0u32,
                start_height: 0u32,
                number_of_interval: 0u32,
                total_amount: 0u128,
                claimed_amount: 0u128,
                token_id: token_id
            });
            let total_amount:u128 = metadata.total_amount + amount;
            assert(start_height > block.height);
            beneficiary_metadata.set(beneficiary, BeneficiaryMetadata {
                block_duration: block_duration,
                start_height: start_height,
                number_of_interval: number_of_interval,
                total_amount: total_amount,
                claimed_amount: metadata.claimed_amount,
                token_id: token_id
            });
        
        }


        async transition claim(access_record: AccessRecord, amount:u128, token_id: field, external_authorization_required: bool) -> (token_registry.aleo/Token, AccessRecord,Future) {
            let (token_record,transfer):(token_registry.aleo/Token, Future)=token_registry.aleo/transfer_public_to_private(token_id, access_record.owner, amount, external_authorization_required);
            let beneficiary_hash_address:address = BHP256::hash_to_address(access_record.owner);
            let new_access_record:AccessRecord = AccessRecord {
                owner: access_record.owner
            };
            return (token_record, new_access_record, finalize_claim(amount, token_id, beneficiary_hash_address, transfer));
        }

        async function finalize_claim (amount:u128, token_id:field, beneficiary:address, transfer:Future) {
            transfer.await();
            let metadata:BeneficiaryMetadata = beneficiary_metadata.get(beneficiary);
            let claimed_amount:u128 = metadata.claimed_amount + amount;
            assert(claimed_amount <= metadata.total_amount);
            assert(block.height > metadata.start_height);
            assert((block.height - metadata.start_height)> (metadata.block_duration / metadata.number_of_interval));
            assert(amount == (metadata.total_amount * metadata.number_of_interval as u128 / metadata.block_duration as u128));
            beneficiary_metadata.set(beneficiary, BeneficiaryMetadata {
                block_duration: metadata.block_duration,
                start_height: metadata.start_height,
                number_of_interval: metadata.number_of_interval,
                total_amount: metadata.total_amount,
                claimed_amount: claimed_amount,
                token_id: metadata.token_id
            });
        }

    }
    ~~~

    </blockquote></details>

## Deploy Project:
- Command:
    ```sh
    leo deploy --network testnet -y
    ```
    <details><summary> Detailed Output </summary><blockquote>

    ~~~sh
       Leo ✅ Compiled 'gcyadav_token_vesting_linear.aleo' into Aleo instructions
    📦 Creating deployment transaction for 'gcyadav_token_vesting_linear.aleo'...

    Base deployment cost for 'gcyadav_token_vesting_linear.aleo' is 13.1182 credits.

    +-----------------------------------+----------------+
    | gcyadav_token_vesting_linear.aleo | Cost (credits) |
    +-----------------------------------+----------------+
    | Transaction Storage               | 5.331000       |
    +-----------------------------------+----------------+
    | Program Synthesis                 | 6.787200       |
    +-----------------------------------+----------------+
    | Namespace                         | 1.000000       |
    +-----------------------------------+----------------+
    | Priority Fee                      | 0.000000       |
    +-----------------------------------+----------------+
    | Total                             | 13.118200      |
    +-----------------------------------+----------------+

    Your current public balance is 15.2469 credits.

    ✅ Created deployment transaction for 'gcyadav_token_vesting_linear.aleo'

    Broadcasting transaction to https://api.explorer.provable.com/v1/testnet/transaction/broadcast...

    ⌛ Deployment at100g5rdfhfehc6mtv4dwd30jvwm605kn59752kct9gw74x2c56uqqggzmhe ('gcyadav_token_vesting_linear.aleo') has been broadcast to https://api.explorer.provable.com/v1/testnet/transaction/broadcast.
    ~~~

    </blockquote></details>

- On-Chain Outputs:
    - Transaction ID: `at100g5rdfhfehc6mtv4dwd30jvwm605kn59752kct9gw74x2c56uqqggzmhe`
    - Aleo Program: [https://testnet.aleo.info/program/gcyadav_token_vesting_linear.aleo](https://testnet.aleo.info/program/gcyadav_token_vesting_linear.aleo)
    - Deploy Txn: [https://testnet.aleo.info/transaction/at100g5rdfhfehc6mtv4dwd30jvwm605kn59752kct9gw74x2c56uqqggzmhe](https://testnet.aleo.info/transaction/at100g5rdfhfehc6mtv4dwd30jvwm605kn59752kct9gw74x2c56uqqggzmhe) 

# Signature:
## Sign with `Transaction ID`:
- For me, program deployed Transaction ID is: `at100g5rdfhfehc6mtv4dwd30jvwm605kn59752kct9gw74x2c56uqqggzmhe`. Command:
    ```sh
    leo account sign -d --private-key <redacted> --message "at100g5rdfhfehc6mtv4dwd30jvwm605kn59752kct9gw74x2c56uqqggzmhe" --raw
    ```
- Output:
    ```sh
    sign17uz2qvycn3vpf3ve8uetanul2sjf49awsy5fm53lzlgyln4yhsq0pn0mewy5xetrvwckzs7j84lxkesuf98j3uafvghlt05lp8nguqrlc397gmr5mrjksfmaqwadgrxa2my23un2zj2ad2yvlh8f97a9qpmh7z2ycgkfzyf9smlq7kafv7wg8pqavp59fmxzqe2hrct9w0kqvljky9s
    ```