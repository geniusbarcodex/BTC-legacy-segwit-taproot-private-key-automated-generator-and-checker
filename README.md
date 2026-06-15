# BTC-legacy-segwit-taproot-private-key-automated-generator-and-checkerr
This program continuously generates Bitcoin private keys and derives their corresponding public addresses. Each generated address is checked against blockchain data to determine whether it has any associated balance or transaction history. Any matching results are logged and saved to a text file within the designated output folder.


For my first result it took 72 days of continuous running the program. If you get any results please make sure to contact me on geniusbar445@proton.me. i would apreciate a small cut if you are to find some dormant wallets.

im working towards creating one for eth chain however it doesnt seem to pick up speed like the btc one. i will go ahead and release once its ready for the world  

RESULTS: 

1. $3,606.69 

hash:
75bd489d4e6ffca2b084b79fe3f3851a73c4949f24db4525986fae14271d6516

legacy address:
bc1qsrqtfmdjlxkzr93m3hhj8vapljrfpq6fakx7mt


2. $234.77
  (requesting for hash and address)


3. 






below is the code that can be run using python script. Create your own folder with the variable name for results. (bit256)



import requests

API_URL = "https://blockstream.info/api/address/"

def get_balance(address):
    try:
        response = requests.get(API_URL + address, timeout=10)
        response.raise_for_status()

        data = response.json()
        funded = data["chain_stats"]["funded_txo_sum"]
        spent = data["chain_stats"]["spent_txo_sum"]

        balance_sats = funded - spent
        balance_btc = balance_sats / 100_000_000

        return balance_btc

    except Exception as e:
        return f"Error: {e}"

with open("addresses.txt", "r") as f:
    addresses = [line.strip() for line in f if line.strip()]

with open("results.txt", "w") as out:
    for address in addresses:
        balance = get_balance(address)
        result = f"{address} : {balance} BTC"
        print(result)
        out.write(result + "\n")
