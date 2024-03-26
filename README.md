# ArtisanChain-Decentralized-Marketplace-for-Handcrafted-Goods
ArtisanChain is a Web3-based platform that connects artisans with customers worldwide, ensuring fair prices and transparent supply chains using blockchain technology.
# ArtisanChain Demo: Decentralized Marketplace for Handcrafted Goods

import hashlib
import time

class Block:
    def __init__(self, index, transactions, timestamp, previous_hash):
        self.index = index
        self.transactions = transactions
        self.timestamp = timestamp
        self.previous_hash = previous_hash
        self.hash = self.calculate_hash()

    def calculate_hash(self):
        block_string = "{}{}{}{}".format(self.index, self.transactions, self.timestamp, self.previous_hash)
        return hashlib.sha256(block_string.encode()).hexdigest()

class Blockchain:
    def __init__(self):
        self.chain = [self.create_genesis_block()]
        self.pending_transactions = []

    def create_genesis_block(self):
        return Block(0, [], time.time(), "0")

    def add_block(self, block):
        block.previous_hash = self.chain[-1].hash
        block.hash = block.calculate_hash()
        self.chain.append(block)

    def add_transaction(self, transaction):
        self.pending_transactions.append(transaction)

    def mine_pending_transactions(self):
        new_block = Block(len(self.chain), self.pending_transactions, time.time(), self.chain[-1].hash)
        self.add_block(new_block)
        self.pending_transactions = []

class ProductListing:
    def __init__(self, artisan_id, product_name, price):
        self.artisan_id = artisan_id
        self.product_name = product_name
        self.price = price

class Marketplace:
    def __init__(self):
        self.listings = []
        self.blockchain = Blockchain()

    def add_listing(self, artisan_id, product_name, price):
        new_listing = ProductListing(artisan_id, product_name, price)
        self.listings.append(new_listing)
        print(f"Added new listing: {product_name} by Artisan {artisan_id}")

    def purchase_product(self, product_name, buyer_id):
        for listing in self.listings:
            if listing.product_name == product_name:
                transaction = {'from': buyer_id, 'to': listing.artisan_id, 'amount': listing.price}
                self.blockchain.add_transaction(transaction)
                print(f"Transaction added for purchasing {product_name}")
                break

    def finalize_transactions(self):
        self.blockchain.mine_pending_transactions()
        print("All pending transactions have been finalized.")

# Example Usage
marketplace = Marketplace()
marketplace.add_listing("Artisan_001", "Handcrafted Wooden Bowl", 100)
marketplace.purchase_product("Handcrafted Wooden Bowl", "Buyer_123")
marketplace.finalize_transactions()

# To view the blockchain and ensure the integrity of transactions
for block in marketplace.blockchain.chain:
    print(f"Block Hash: {block.hash}")
