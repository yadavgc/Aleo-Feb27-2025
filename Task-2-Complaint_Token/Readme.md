# KYC Complaint Token:

## Create leo Project:
- Command:
    ```sh
    leo new gcyadav_kyc_complaint_token
    ```

## Install dependencies:
- Command:
    ```sh
    cd gcyadav_kyc_complaint_token
    leo add token_registry
    leo add credits
    ```

## Compiled Program:
- Command:
    ```sh
    leo build
    ```

## Deploy to testnet:
- Value of Endpoint must be changed with: `https://api.explorer.provable.com/v1` in `.env` file.
- Now, the value of `PRIVATE_KEY` in `.env` must be replaced.
- We will require some token to deploy our program into testnet.

- Command:
    ```sh
    leo deploy --network testnet -y
    ```
    
    <details><summary> Detailed Output </summary><blockquote>

    ~~~sh
       Leo ✅ Compiled 'gcyadav_kyc_complaint_token.aleo' into Aleo instructions
    📦 Creating deployment transaction for 'gcyadav_kyc_complaint_token.aleo'...


    Base deployment cost for 'gcyadav_kyc_complaint_token.aleo' is 10.1939 credits.

    +----------------------------------+----------------+
    | gcyadav_kyc_complaint_token.aleo | Cost (credits) |
    +----------------------------------+----------------+
    | Transaction Storage              | 4.418000       |
    +----------------------------------+----------------+
    | Program Synthesis                | 4.775900       |
    +----------------------------------+----------------+
    | Namespace                        | 1.000000       |
    +----------------------------------+----------------+
    | Priority Fee                     | 0.000000       |
    +----------------------------------+----------------+
    | Total                            | 10.193900      |
    +----------------------------------+----------------+

    Your current public balance is 10.251709 credits.

    ✅ Created deployment transaction for 'gcyadav_kyc_complaint_token.aleo'

    Broadcasting transaction to https://api.explorer.provable.com/v1/testnet/transaction/broadcast...

    ⌛ Deployment at1nt6ttrmk7j6jxz54un4zspmjd0u9twuqzq3seuqy0k9cg2eygugslfcces ('gcyadav_kyc_complaint_token.aleo') has been broadcast to https://api.explorer.provable.com/v1/testnet/transaction/broadcast.
    ~~~

    </blockquote></details>

- On-Chain Outputs: 
    - Transaction ID: `at1nt6ttrmk7j6jxz54un4zspmjd0u9twuqzq3seuqy0k9cg2eygugslfcces`
    - Aleo Program: [https://testnet.aleo.info/program/gcyadav_kyc_complaint_token.aleo](https://testnet.aleo.info/program/gcyadav_kyc_complaint_token.aleo)
    - Deploy Txn: [https://testnet.aleo.info/transaction/at1nt6ttrmk7j6jxz54un4zspmjd0u9twuqzq3seuqy0k9cg2eygugslfcces](https://testnet.aleo.info/transaction/at1nt6ttrmk7j6jxz54un4zspmjd0u9twuqzq3seuqy0k9cg2eygugslfcces) 

# Signature:
## Sign with `Transaction ID`:
- For me, program deployed Transaction ID is: `at1nt6ttrmk7j6jxz54un4zspmjd0u9twuqzq3seuqy0k9cg2eygugslfcces`. Command:
    ```sh
    leo account sign -d --private-key <redacted> --message "at1nt6ttrmk7j6jxz54un4zspmjd0u9twuqzq3seuqy0k9cg2eygugslfcces" --raw
    ```
- Output:
    ```sh
    sign1fx2gdwn8sc9ywc5sn6q4z6ejtf82gqfklx2k0wvw5c5paxd42cqwtqz2n7qqg9xfg3rkpkq5hc6wc6pcuucdlemzlaephl23vgxauqmlc397gmr5mrjksfmaqwadgrxa2my23un2zj2ad2yvlh8f97a9qpmh7z2ycgkfzyf9smlq7kafv7wg8pqavp59fmxzqe2hrct9w0kqvq9gej8
    ```