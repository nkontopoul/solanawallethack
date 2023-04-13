# solanawallethack

Attempt for a  a Python script to test a list of phrases as potential Solana wallet seeds, and then check and print the ones with a positive balance. The Python script below uses the solana-py library. If you haven't installed it yet, run:


pip install solana

Then:


import base58
from solana.rpc.api import Client
from solana.wallet import Wallet
from solana.wallet.derive import derive_wallet

# Replace the list of phrases with the ones you want to test
phrases_to_test = [
    "phrase 1",
    "phrase 2",
    "phrase 3",
]

# Set up Solana client
solana_client = Client("https://api.mainnet-beta.solana.com")

def check_wallet_balance(wallet: Wallet):
    account_info = solana_client.get_account_info(wallet.public_key)
    if account_info:
        lamports = account_info["lamports"]
        sol_balance = lamports / 1e9
        return sol_balance
    return 0

def main():
    for phrase in phrases_to_test:
        try:
            wallet = derive_wallet(phrase)
            balance = check_wallet_balance(wallet)
            if balance > 0:
                print(f"Phrase: {phrase}\nAddress: {wallet.public_key}\nBalance: {balance} SOL\n")
        except ValueError:
            print(f"Invalid phrase: {phrase}")

if __name__ == "__main__":
    main()
