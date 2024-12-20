---
title: Python SDK Setup
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Prerequisite

For running the tutorial for developers, we require Python 10 or later version and pip 24 to be installed in your environment. To install python and pip, we recommend you go to the [Python official website](https://www.python.org/) and download an appropriate version.

If you want to upgrade python to 10 or higher version, we recommend you to use [pyenv](https://github.com/pyenv/pyenv). If you want to upgrade pip to the latest version, you can run the command `pip install --upgrade pip`.

# Create a Python Project

Create a folder to host the node project called `story-python-example`:

```Text Shell
mkdir story-python-example
```

Go into the folder:

```Text Shell
cd story-python-example/
```

# Install the Dependencies

In the current folder `story-python-example`, install the Story Protocol Python SDK package, as well as `web3` ([https://pypi.org/project/web3/](https://pypi.org/project/web3/)) to access the DeFi wallet accounts.

```python pip
pip install story_protocol_python_sdk web3
```

# Initiate SDK Client

Next we can initiate the SDK Client by first setting up our account and then the client itself.

## Set Up Private Key Account

There are two ways to set up an account. The first is to use a private key locally:

> :information_source: Make sure to have WALLET\_PRIVATE\_KEY set up in your .env file.
>
> :information_source: Make sure to have RPC\_PROVIDER\_URL for your desired chain set up in your .env file. You can use a public default one (`RPC_PROVIDER_URL=https://rpc.odyssey.storyrpc.io`) or check out other RPCs here.

```python main.py
import os
from dotenv import load_dotenv
from web3 import Web3

from story_protocol_python_sdk import StoryClient

load_dotenv()
private_key = os.getenv('WALLET_PRIVATE_KEY')
rpc_url = os.getenv('RPC_PROVIDER_URL')

# Initialize Web3
web3 = Web3(Web3.HTTPProvider(rpc_url))

# Set up the account with the private key
account = web3.eth.account.from_key(private_key)

# Create StoryClient instance
story_client = StoryClient(web3, account, 1516)
```

## Set Up JSON-RPC Account (ex. Metamask)

The second way is to delay signing to a JSON-RPC account like Metamask. This requires creating a request that allows communication between your frontend and your Python backend. You can then use the account obtained from this request in a manner similar to the private key setup described above.