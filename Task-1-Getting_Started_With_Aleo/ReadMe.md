# Getting Started with ALEO:

## Create new project:
- Command:
    ```sh
    leo new gcyadav_leo_bidder_auction
    ```

- Create 3 accounts. Owner, Bidder1, Bidder2.
- Command:
    ```sh
    snarkos account new
    ```
- So, run above command 3 times in terminal:
    <details><summary> Detailed Output </summary><blockquote>

    ~~~
    $ snarkos account new

    Private Key  <redacted>
        View Key  <redacted>
        Address  aleo1yhvl35uwjcy72hmacufn4h08ew7n7r97zq48vegqefuqtewh4grspu7gqk

    $ snarkos account new

    Private Key  <redacted>
        View Key  <redacted>
        Address  aleo1cj9vcgknthw6dzs042ys9gxcr3ar095h80j7k37w7zxq69ycpsgqjencwa

    $ snarkos account new

    Private Key  <redacted>
        View Key  <redacted>
        Address  aleo1m8mr8yq0nnfssfgyvcx48g7vdahprxc9rt5gt66e0dksvg7ktvxsr7euyq
    ~~~
    
    </blockquote></details>

- In above output, suppose 1 a/c with Address `aleo1yhvl35uwjcy72hmacufn4h08ew7n7r97zq48vegqefuqtewh4grspu7gqk` is owner and remaining 2nd and 3rd a/cs are bidders. 
- Make sure `ENDPOINT` in `.env` file is `https://api.explorer.provable.com/v1`.
     
## Deploy To Testnet:
- We need ALEO token in each accounts. So load up before moving any further.
- Now, the value of `PRIVATE_KEY` in `.env` must be replaced with the `PRIVATE_KEY` of the Owner.
- Command:
    ```sh
    leo deploy --network testnet -y
    ```
    <details><summary> Detailed Output </summary><blockquote>

    ~~~
       Leo ✅ Compiled 'gcyadav_leo_bidder_auction.aleo' into Aleo instructions
        📦 Creating deployment transaction for 'gcyadav_leo_bidder_auction.aleo'...


        Base deployment cost for 'gcyadav_leo_bidder_auction.aleo' is 14.47925 credits.

        +---------------------------------+----------------+
        | gcyadav_leo_bidder_auction.aleo | Cost (credits) |
        +---------------------------------+----------------+
        | Transaction Storage             | 3.638000       |
        +---------------------------------+----------------+
        | Program Synthesis               | 9.841250       |
        +---------------------------------+----------------+
        | Namespace                       | 1.000000       |
        +---------------------------------+----------------+
        | Priority Fee                    | 0.000000       |
        +---------------------------------+----------------+
        | Total                           | 14.479250      |
        +---------------------------------+----------------+

        Your current public balance is 16.1287 credits.

        ✅ Created deployment transaction for 'gcyadav_leo_bidder_auction.aleo'

        Broadcasting transaction to https://api.explorer.provable.com/v1/testnet/transaction/broadcast...

        ⌛ Deployment at1qj4vlwjxul9e0ev380v0x28ze703upjmmtzv7ae06p4k9f6dj59q0egch5 ('gcyadav_leo_bidder_auction.aleo') has been broadcast to https://api.explorer.provable.com/v1/testnet/transaction/broadcast.

    ~~~

    </blockquote></details>


- On-Chain Outputs: 
    - Deployment ID: `at1qj4vlwjxul9e0ev380v0x28ze703upjmmtzv7ae06p4k9f6dj59q0egch5`
    - Aleo Program: [https://testnet.aleo.info/program/gcyadav_leo_bidder_auction.aleo](https://testnet.aleo.info/program/gcyadav_leo_bidder_auction.aleo)
    - Deploy Txn: [https://testnet.aleo.info/transaction/at1qj4vlwjxul9e0ev380v0x28ze703upjmmtzv7ae06p4k9f6dj59q0egch5](https://testnet.aleo.info/transaction/at1qj4vlwjxul9e0ev380v0x28ze703upjmmtzv7ae06p4k9f6dj59q0egch5) 


## Execution Details:
- 1st Bidder Execution:
    - Transition ID: `at1c9krgz93z5q8zxw3ay0fh3x8hfk4l7428qxfv83c0zu057ehuqyqxzdzx8`
    - Output Record: `record1qyqsqv6g9xvnecxnxwpvckkr7fns4l662yc7rumccapd3czgfzxxegg0qvrxy6tyv3jhyscqqgpqpm9h06clyfrdqm7yfvcpfs79lquklz3vl5tm9ynq62mdr6srygcj22dj6x8rv2en4hm8ppv75t2h9jv2l0eyx7me76n6lnax28qu5ygqvctdda6kuaprqqpqzq9nhnnvy8zjg34zy79amzg0707fz7k3dy4ea4wsj50eg0wevrlazqykju6lwa5kumn9wg3sqqspqpmkfy2sruc4q46rthn9qmkrgzx797tndgzrpz97td78354tqwuqr8dg803kz7df4yzuawuw8qcarqdkhv5zdus39zezw3q4jhae87qxy5g7ms`

- 2nd Bidder Execution:
    - Transition ID: `at16nz4sjm7edpt066s50fmy9swgy2zuf8xamd054jsuf34akts9uzqf0u385`
    - Output Record: `record1qyqsqwwtdg0jtavy22mv72lzr688fusn8nftvnqlzq3srgzq3q00w6qdqvrxy6tyv3jhyscqqgpqpu7qy2klmxm2rfxvr3s9tfv528g4n53806x632uwnwu8ze4j3xqjx4z6kzx7vldzhdx3h5kwp33hyd4vec6hre7znzp2szqud9dctcgsvctdda6kuaprqqpqzqrf479u585gdjym8anquhke7tcakugpnkmw2hhdhu87l7jcuvsspqykju6lwa5kumn9wg3sqqspqzguz3wqqt0dz6uecadxz0scxmurdqhdglvjvwzsl2c5tqhzm6usy7gwye7wh28eeddq4rtpv930uqrs6wkqglkqa788yrgv5ke4keg8ny3ewv`

- Decrypt Record using:
    ```sh
    snarkos developer decrypt -v <VIEW_KEY> -c <RECORD_CIPHERTEXT>
    ```

- Resolve Bid:
    - Transition ID: `at1gynz5afqchp7n7x0qpx8rqq60ed46ye9zqeftl4kwwz3cw2e6uzqn72qn4`
    - Output Record: `record1qyqsq5my3dx5qt6wvsecaq9tswq4q698a44xe8yrk4e7gd2vu53ayrcpqvrxy6tyv3jhyscqqgpqpdkj7qrwhjcqny0d3dtzsz8q6weyujcdh7aem0hr08lk9524jtqgpqt2crvgd2hf8p2npt4qcvmnjj8smaqt8a7qjum9hg9ym5h24cqqvctdda6kuaprqqpqzqz500lx5d5l809u3ppw4sxreq268c4e3x4gkqcywkp6nt52p8gjqqykju6lwa5kumn9wg3sqqspqqhhkd8xa06zsaxrpa8lm74tds9du0hf57eq8vrehmzvkjrzth33qzjm4ae7wzg0uev8df9ngdc5fv7g74pkhyka0zh3g0smaag40lqtx5w823`

- Finish Bid:
    - Transition ID: `at1dl2nu59eucd04ggdl9du6wstgh38azt6ssheqh9fgsu7ca8ypqzsysy6mn`
    - Output Record: `record1qyqspxwc8xls7w0luf8gt2qmfy87jz0uy8fue80vvqkjxgwmfq378jcwqvrxy6tyv3jhyscqqgpqp5e7h2pe90u9amge8tx2p7m5gsuqk6nysdcwymp0sctcp6tr30cxvxyytp460580n9ef5ndxq45cwljmldglm3whdq6me9zd5zd29yyqvctdda6kuaprqqpqzqpvtjqdzl57k60kufhdsug4q647mdnueyf5nx640hs8h9u64z04qgykju6lwa5kumn9wg3sqqspqpuh64y5qw33p7hav7tnk7hfpynxt3r90f2yhg0qnw5g045m4dvq82h9ng4q5gqtsrwah8ktqwy6ewsllcrz5470mcvfl4ur28azwsqx2d0ukz`

- Claim Bid:
    - Transition ID: `at1rzkvzfu56uf2wsxyzt5zag0msmdrpp34h9yqdyeffdnp5j79xq8qp87gew`
    - Output Record: `record1qyqsp3yvulhumh3e06ztjmkajaq8gcmk9f457rajsg9ugcpqnyfaaysgqyrxzmt0w4h8ggcqqgqsp3nhcyhzyjzxaesc7v3g9c4ccw77l3q3uy6xmhtah5t4d9wl7lcwz0ajlqw37lf6fjeez6wg3d84g8mnt94vgxg0jxcdvq8uhpar4vxs0p39mp`

# Signature:
## Sign with `true`:
- Command:
    ```sh
    leo account sign --private-key <redacted> --message true
    ```

- Output:
    ```sh
    sign1wgns3vfl28u00l8hm4nyzsg97vx2u3wkqa7xa0ltagsehqd5rsppr6l6zsj4gqvczkspy49vv680vxwcs6a298krdp3g4ecu7qvxzprlc397gmr5mrjksfmaqwadgrxa2my23un2zj2ad2yvlh8f97a9qpmh7z2ycgkfzyf9smlq7kafv7wg8pqavp59fmxzqe2hrct9w0kqvcydynf
    ```

## Sign with `Transaction ID`:
- For me, program deployed Transaction ID is: `at1qj4vlwjxul9e0ev380v0x28ze703upjmmtzv7ae06p4k9f6dj59q0egch5`. Command:
    ```sh
    leo account sign -d --private-key <redacted> --message "at1qj4vlwjxul9e0ev380v0x28ze703upjmmtzv7ae06p4k9f6dj59q0egch5" --raw
    ```
- Output:
    ```sh
    sign1pg57nxgs2xj2f7x0yzjx7zz7zfdr825686stl2hy7dnh7vlf6ypd9c6djjafn6an4j8jdgfrgpsavr8dqn2qrzc9pghqu9zqqhu4jqtlc397gmr5mrjksfmaqwadgrxa2my23un2zj2ad2yvlh8f97a9qpmh7z2ycgkfzyf9smlq7kafv7wg8pqavp59fmxzqe2hrct9w0kqveluj5k
    ```