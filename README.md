# Mech tool template

A template for development of mech tools on the [Olas Network](https://olas.network/). Find the documentation [here](https://docs.autonolas.network).


## What is a mech?

A mech is an autonomous service that listens for on-chain requests and performs the needed actions in exchange for a small payment. These requests are usually LLM requests (although they can be other generic jobs), and their metadata is stored on IPFS while its hash is written to a smart contract that also handles the payment. We can think of a mech as an on-demand brain for your applications.

## Why do we need mechs?

Mechs implement different AI-oriented tools and pay for private API access like OpenAI API. Mechs act as a central hub or library where your applications can make LLM requests and avoid having to pay for multiple APIs or implementing different API interfaces. Think of it as a generic interface to multiple LLMs and smart tools.

## What does the mech request-response flow look like?

![Mech request-response flow](/images/mech_request.png)

## System requirements

- Python `>=3.10`
- [Pip](https://pip.pypa.io/en/stable/installation/)
- [Poetry](https://python-poetry.org/)


## How to run the local tool examples

Create a virtual environment with all development dependencies:

    poetry shell
    poetry install


Run the demo calculator tool:

    python packages/valory/customs/calculator_request/calculator_request.py

Run the prediction request tool:

    export OPENAI_API_KEY=<your_openai_key>
    python packages/valory/customs/prediction_request/prediction_request.py


## How to interact with already deployed mech tools

1. Prepare your private key. You can either export an already existing key from wallets like Metamask and save it to a file called `ethereum_private_key.txt`:

    ```bash
    echo -n YOUR_PRIVATE_KEY > ethereum_private_key.txt
    ```

    Or create a new key (you will need to send some funds) by running:

    ```bash
    aea generate-key ethereum
    ```

2. Ensure that you have some funds in your wallet (i.e 0.05 xDAI if you're running on Gnosis Chain)

3. Send a mech request using the mech cli:

    ```bash
    mechx interact "write a short poem" 6 --key ethereum_private_key.txt --tool openai-gpt-3.5-turbo --chain-config gnosis --confirm on-chain
    ```

4. Send another mech request programmatically:

    ```bash
    python scripts/mech_request.py
    ```


## Develop your own tool

1. Create your tool under `packages/<your_author_handle>/customs/<your_tool_name>` using the examples as reference.

2. Calculate your tool hashes. The first time you run this command you will be asked to add your tool to either `dev` or `third_party` packages section. Use `dev`:

    ```
    autonomy packages lock
    ```

3. Test your tool running it as a Python script and ensuring it does what it is intented to do.

4. Open a PR against the [mech repository](https://github.com/valory-xyz/mech) and tag [an engineer](https://github.com/valory-xyz/mech/graphs/contributors) for review.

5. Once your PR has been approved, merged and deployed to the Mech, you will be able to interact with your tool using the mech-client.